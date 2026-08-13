# QUARKUS-8368 - Upgrade to Hibernate ORM 7.4, Reactive 3.4, Search 8.4, Elasticsearch 9.4

JIRA: https://issues.redhat.com/browse/QUARKUS-8368

Upstream PR: https://github.com/quarkusio/quarkus/pull/54083

## Scope of the testing

With the upgrade of Hibernate ORM, Reactive, and Search, we need to ensure that changes are properly handled by our Quarkus test suite
and work as expected.

Hibernate Reactive 3.4 and Search 8.4 are alignment releases, backward-compatible with their previous versions. No test development
is needed for either; existing test coverage validates backward compatibility.

### What will be tested

#### Hibernate ORM
- ***Pagination with collection fetch joins:***
  - Verify that HQL queries combining pagination (`setFirstResult`/`setMaxResults`) with `JOIN FETCH` on `@OneToMany` collections
  return correct results, validating the ORM 7.4 behavioral change from in-memory to SQL-level limit processing.
- ***Temporal entity support:***
  - Verify that a basic `@Temporal` entity can be persisted, updated, and queried for both current state and historical state
  via time-travel (`SessionFactory.withOptions().asOf()`), validating the Quarkus integration with ORM 7.4's
  `ChangesetCoordinatorInitiator` in the blocking service registry. The feature is still incubating, hence the added test is simple
  and only asserts principle.

### What will not be tested

- **`@Audited` entity support:** incubating state
- **`@Temporal` on Hibernate Reactive:** incubating state 

## Existing test coverage

The existing Quarkus QE Test Suite, specifically within the `hibernate/` modules, has extensive test coverage that will also be executed to ensure backward compatibility.

## Impact on test suites and testing automation

New integration tests will be added to the existing module:
- `hibernate/hibernate-orm`

## Impact on resources

Negligible impact. The test execution time will increase slightly:
- JVM mode: ~2 minutes additional
- Native mode: ~5 minutes additional
- OCP: ~3 minutes for JVM mode and ~8 minutes for native mode.

## Getting familiar with the feature

  - [#54083](https://github.com/quarkusio/quarkus/pull/54083)
  - [Hibernate ORM 7.4 What's New](https://docs.hibernate.org/orm/7.4/whats-new/whats-new.html) and [Migration Guide](https://docs.hibernate.org/orm/7.4/migration-guide/)
  - [Quarkus Hibernate ORM guide](https://quarkus.io/guides/hibernate-orm-panache)

## Contacts

* Tester: Jose Carranza <jcarranz1@ibm.com>
