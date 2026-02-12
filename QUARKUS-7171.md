# QUARKUS-7171 - Upgrade to Hibernate ORM 7.2, Reactive 3.2, Search 8.2, Elasticsearch 9.2 / OpenSearch 3.3

JIRA: https://issues.redhat.com/browse/QUARKUS-7171

Upstream PR: https://github.com/quarkusio/quarkus/pull/50519

## Scope of the testing
With the upgrade of Hibernate ORM, Reactive, and Search, we need to ensure that changes are properly handled by our Quarkus test suite
and work as expected.

### What will be tested

#### Hibernate ORM
- ***Flush event handling:***
  - Verify that session event listeners are correctly triggered even during no-op flushes (flushes where no database changes occur), validating
  the new flush operations added (e.g., Empty session flush)
- ***Session API enhancements:***
  - Verify new Quarkus implementation of Session transaction management helper methods (e.g., `Session.inTransaction()` , `Session.fromTransaction()` ) to ensure correct delegation in Quarkus sessions.
- ***HQL Regular Expressions***
  - Verify support for the new `like regexp` and `ilike regexp` operators introduced in Hibernate ORM 7.2.To ensure these operators are correctly parsed and executed.
#### Spring Data JPA
- ***Strict type validation:***
  - Verify that derived query methods throw `QueryArgumentException` with improved error messages when incompatible types are passed (e.g.,
  passing a String instead of an Enum).
#### Hibernate Reactive
- ***Panache exception handling:***
  - Verify that operations on unmanaged entities now throw `IllegalArgumentException` instead of `PersistenceException`, aligning with the
  Hibernate Reactive specification.

## Existing test coverage

The existing Quarkus QE Test Suite, specifically within the `hibernate/` modules and `spring/spring-data` module, has extensive test coverage across several
sub-modules that will also be executed to ensure backward compatibility.

## Impact on test suites and testing automation

New integration tests will be added to the existing modules:
- `hibernate/hibernate-orm`
- `hibernate/hibernate-reactive`
- `spring/spring-data`

ElasticSearch will be updated in our quarkus-test-suite to `9.2.2`

## Impact on resources

Negligible impact. The test execution time will increase slightly:
- JVM mode: ~2 minutes additional
- OCP: ~3 minutes for JVM mode and ~5 minutes for native mode.
- Native mode: ~8 minutes additional

## Getting familiar with the feature

- Review upstream PR [Upstream PR #50519](https://github.com/quarkusio/quarkus/pull/50519) and analyze all modified test files
- Quarkus Hibernate guides:
  * [hibernate-orm-panache](https://quarkus.io/guides/hibernate-orm-panache)
  * [hibernate-search-orm-elasticsearch](https://quarkus.io/guides/hibernate-search-orm-elasticsearch)
  * [hibernate-reactive](https://quarkus.io/guides/hibernate-reactive)
- Quarkus Spring:
    * https://quarkus.io/guides/spring-data-jpa
- Hibernate guides : 
  * [Hibernate ORM 7.2 What's New](https://docs.hibernate.org/orm/7.2/whats-new/)
  * [Hibernate ORM 7.2 Migration Guide](https://docs.hibernate.org/orm/7.2/migration-guide/)
  * [Hibernate Reactive 3.2 Documentation](https://hibernate.org/reactive/releases/3.2/) 

## Contacts

* Tester: Jose Carranza <jcarranz1@ibm.com>
