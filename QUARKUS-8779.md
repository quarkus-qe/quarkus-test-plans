# QUARKUS-8779 - Support weight-based eviction for Hibernate 2LC Caffeine cache regions

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8779

Upstream issue: https://github.com/quarkusio/quarkus/issues/55282

Upstream PR: https://github.com/quarkusio/quarkus/pull/55284

Follow-up issue: https://github.com/quarkusio/quarkus/issues/55336

Validation issue: https://github.com/quarkusio/quarkus/issues/57767

Validation PR: https://github.com/quarkusio/quarkus/pull/57777

Documentation: https://quarkus.io/guides/hibernate-orm#caching

Hibernate's second-level cache in Quarkus is backed by Caffeine, and until now only count-based eviction was 
exposed (`memory.object-count`). This feature adds two per-region properties that switch a region to 
Caffeine's weight-based eviction:

```properties
quarkus.hibernate-orm.cache."org.acme.MyEntity".memory.maximum-weight=104857600
quarkus.hibernate-orm.cache."org.acme.MyEntity".memory.weigher-class=org.acme.MyEntityWeigher
```

The motivation is entities with highly variable sizes (JSON/BLOB payloads), where `object-count` cannot bound memory:
10000 entries may be 10 MB or 200 GB.

## Scope of the testing

In scope:

* Weight-based eviction actually bounding a region, and the same workload under `object-count` not evicting.
  Upstream never asserts eviction, only configuration values.
* Usability of writing a *meaningful* weigher — one deriving a weight from the value Hibernate actually caches,
  rather than returning a constant.
* Failure modes build-time validation does not catch: `maximum-weight` without `weigher-class`, negative and zero
  `maximum-weight`, and a `weigher-class` that does not resolve, does not implement `Weigher`, or has no public
  no-arg constructor. Covered upstream, see Scenario 2.
* Named persistence units, and coexistence of count-based and weight-based regions in the same persistence unit.
* Interaction with `expiration.max-idle` on a weight-based region.

Out of scope:

* Build-time validation of the two mutually exclusive combinations. Upstream covers both.
* Memory-footprint and startup benchmarking.
* Weigher auto-discovery and a built-in default weigher — proposed in
  [#55336](https://github.com/quarkusio/quarkus/issues/55336), not implemented in this iteration. If implemented, the 
  test scenarios will need revisiting.
* Non-Caffeine 2LC providers — the properties are wired into the Caffeine cache manager only.

Modes: JVM and native.

## Extension interactions and integration points

* **Panache / Jakarta Data / stateless sessions** consume the 2LC transparently, but the module's existing
  stateless-session cache tests share the persistence unit, so a misconfigured region surfaces as an unrelated
  regression.
* **Hibernate Reactive — confirmed to share the code path.** It does not merely share the `quarkus.hibernate-orm.*`
  config root: its persistence provider constructs the same Caffeine cache manager from the same configuration
  object as the ORM one.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed the upstream issue, the pull request, and the follow-up issue.
* Reviewed the configuration mapping, build-time validation, and the cache manager's
  weigher instantiation on the Quarkus side, and the weight/weigher contract on the Caffeine side.
* Established whether Hibernate Reactive shares the cache manager by comparing the two persistence providers.
* Reviewed the upstream unit and integration tests for the feature, and the new documentation section.
* Reviewed existing QE coverage in `hibernate/hibernate-orm` and `cache/caffeine`, and the related plans
  [QUARKUS-6261](QUARKUS-6261.MD) and [QUARKUS-7820](QUARKUS-7820.md).

## Existing test coverage

Upstream unit tests assert the two build-time validation messages. A third configures `maximum-weight` with a
weigher and asserts only that an entity is present in the cache; the weigher returns a constant `100` and is never
shown to influence eviction. The `hibernate-orm-cache` integration tests read back the configured maximum weight and
whether a weigher is present.

In the QE suite there is no 2LC memory coverage.

## Planned coverage

Scenarios 1 and 3 extend `hibernate/hibernate-orm`: it already owns a cacheable entity in a named persistence
unit and already runs the full matrix. A new cacheable entity with a large variable-length `data` column is added, 
together with a weigher deriving weight from the cached value and an endpoint exposing the region's maximum weight, 
current weighted size and whether a given id is still cached.

1. **Weight-based eviction bounds the region** — persist and load entries whose combined weight exceeds the bound;
   assert via Awaitility (Caffeine eviction is amortized, not synchronous) that the weighted size stays at or below
   `maximum-weight` and the earliest entries are gone, while the entry count alone would not have triggered
   count-based eviction.

   *Note:* which entries survive is not asserted. Caffeine's admission policy evicts by access
   frequency, not insertion order, and for this workload it tends to keep the earliest entries. Forcing reads on
   the latest entries only changes that past a threshold set by Caffeine internals. The test instead asserts that
   the region is non-empty, retains no more entries than fit in `maximum-weight`, and that its weighted size
   matches the weigher's per-entry weight.

2. **Configuration values build-time validation does not reject** — upstream validation covers only the two
   mutually exclusive combinations, so each value below reaches persistence-unit bootstrap.

   *Invalid `weigher-class`* — a name that does not resolve, a class not implementing `Weigher`, and one without a
   public no-arg constructor.

   *Boundary `maximum-weight` values* — `maximum-weight` without `weigher-class`, a negative value, and zero.

   *Moved upstream.* Testing these cases exposed two bugs, reported in
   [#57767](https://github.com/quarkusio/quarkus/issues/57767): a negative `maximum-weight` silently fell back to
   count-based eviction, and `maximum-weight` without `weigher-class` failed at startup although the documentation
   promised a default weight of 1.

3. **Mixed regions and expiration** — one region on `object-count` and one on `maximum-weight` in the same unit, with
   `expiration.max-idle` set explicitly on the weight-based one. Verify each evicts by its own policy and that idle
   expiration still applies on top of the weight bound.

4. **Hibernate Reactive smoke test** — a single weight-based region in `hibernate/hibernate-reactive`, asserting the
   region is weight-bounded and evicts.

## Impact on test suites and testing automation

* No new module and no new CI job. `hibernate/hibernate-orm` already runs in the JVM, native and OpenShift
  pipelines against PostgreSQL and MySQL; scenario 4 adds one test to the existing `hibernate/hibernate-reactive`
  module.

## Impact on resources

The added tests should have **low to moderate impact** on resources:

* An estimated execution time increase of 5 to 6 minutes.
* Tests will be executed on baremetal and OpenShift, in JVM and native mode.

## Contacts

* Tester: Roberto Cortez <radcortez@ibm.com>

## References

* https://quarkus.io/guides/hibernate-orm#caching
* https://github.com/quarkusio/quarkus/issues/55282
* https://github.com/quarkusio/quarkus/pull/55284
* https://github.com/quarkusio/quarkus/issues/55336
* https://github.com/quarkusio/quarkus/issues/57767
* https://github.com/quarkusio/quarkus/pull/57777
* https://github.com/ben-manes/caffeine/wiki/Eviction#size-based
