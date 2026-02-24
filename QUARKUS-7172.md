# QUARKUS-7172 - Allow setting MariaDB/MySQL storage engine differently for each persistence unit in Hibernate ORM

JIRA: https://issues.redhat.com/browse/QUARKUS-7172

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/49902

## Scope of the testing
Previously, the MariaDB/MySQL storage engine was a global system property shared across all persistence units.
This change makes it a per-PU configuration property, allowing different engines (InnoDB, MyISAM) per persistence unit.

### What will be tested

- **Persistent Unit storage engine configuration:**
    - Verify that multiple Persistent Units connected to MariaDB and MySQL can be configured with
      different storage engines simultaneously and `ConfigurationException` is not already present. 
    
- **Offline Dialect Resolution (`database.start-offline=true`):**
    - Verify that the correct storage engine is applied to each persistent unit by inspecting the dialect (`Dialect.getTableTypeString()`) without starting the database.
- **DDL Verification :**
    - Verify that the generated database schema actually respects the configured engine by starting the database and executing a query to inspect the physical tables.

## Existing test coverage
 - Upstream unit tests ([DialectSpecificSettingsMariaDBTest](https://github.com/quarkusio/quarkus/blob/main/extensions/hibernate-orm/deployment/src/test/java/io/quarkus/hibernate/orm/config/dialect/DialectSpecificSettingsMariaDBTest.java) , [StorageSpecificMysqlDBTest](https://github.com/quarkusio/quarkus/blob/main/extensions/hibernate-orm/deployment/src/test/java/io/quarkus/hibernate/orm/config/dialect/StorageSpecificMysqlDBTest.java)) verify the engine.
 - There is currently no multi-Persistent Unit storage engine coverage in the `quarkus-qe-testsuite`.

## Impact on test suites and testing automation

New integration tests will be added to the existing module:
- `hibernate/hibernate-offline-startup`

## Impact on resources

Negligible impact. The tests will reuse existing MariaDB/MySQL containers.
- JVM mode: ~1-2 minutes additional.
- Native mode: ~3-5 minutes additional.

## Getting familiar with the feature

- Review upstream PRs [49902](https://github.com/quarkusio/quarkus/pull/49902) and
[10873](https://github.com/hibernate/hibernate-orm/pull/10873).
- Quarkus guides:
    * [hibernate-orm](https://quarkus.io/guides/hibernate-orm) 

## Contacts
* Tester: Jose Carranza <jcarranz1@ibm.com>