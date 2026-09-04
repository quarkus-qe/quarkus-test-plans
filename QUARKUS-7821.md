# QUARKUS-7821 - Support @Transactional for Hibernate Reactive

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7821

Upstream PR: https://github.com/quarkusio/quarkus/pull/51063

Documentation: https://quarkus.io/guides/hibernate-reactive/#using-transactional-with-hibernate-reactive

The new `quarkus-reactive-transactions` extension (Preview) enables `@jakarta.transaction.Transactional` on Hibernate Reactive methods returning `Uni<T>`.
Only `TxType.REQUIRED` is supported. This also introduces a breaking change: `@Transactional` no longer forces worker thread dispatch; dispatch is decided by return type.

## Scope of the testing

### What will be tested

* A REST endpoint with `@Transactional` and injected `Mutiny.Session` persists an entity: verify transaction commits on success and rolls back on failure.
* A `@Transactional` method returning `Uni<T>` runs on the Vert.x event loop; adding `@Blocking` restores worker thread dispatch.
* Other `TxType` values (`REQUIRES_NEW`, `MANDATORY`, etc.) throw `UnsupportedOperationException`.
* `Mutiny.StatelessSession` injection within a `@Transactional` method works for CRUD.
* A `@Transactional` method returning `Multi` produces a runtime error.
* Multiple concurrent `@Transactional` requests maintain transaction isolation.

### What will not be tested

* `Uni.combine()`/`Uni.join()` context leakage ([#52815](https://github.com/quarkusio/quarkus/issues/52815)): known, documented limitation.
* XA transactions: documented as unsupported.

## Existing test coverage

The QE module `hibernate/hibernate-reactive` has no existing endpoint using `@Transactional`; all reactive transactions use `Panache.withTransaction()` or `@WithSession`.

## Impact on test suites and testing automation

New tests will be added to the existing `hibernate/hibernate-reactive` module. No new CI jobs needed.

## Impact on resources

Tests will run in JVM, native, and OpenShift.

* Baremetal JVM mode: ~3 minutes additional.
* Baremetal native mode: ~6 minutes additional.
* OCP JVM mode: ~5 minutes additional.
* OCP native mode: ~10 minutes additional.

## Getting familiar with the feature

* Reviewed upstream [PR #51063](https://github.com/quarkusio/quarkus/pull/51063).
* https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.35#quarkus-rest
* https://quarkus.io/guides/hibernate-reactive/

*  Related upstream issues: 
[#52815](https://github.com/quarkusio/quarkus/issues/52815),
[#53570](https://github.com/quarkusio/quarkus/issues/53570),
[#53622](https://github.com/quarkusio/quarkus/issues/53622)

## Contacts

* Tester: Jose Carranza <jcarranz1@ibm.com>
