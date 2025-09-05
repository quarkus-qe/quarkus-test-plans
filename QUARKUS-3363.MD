# QUARKUS-3363 - Offline startup for Hibernate ORM in Quarkus

JIRA: https://issues.redhat.com/browse/QUARKUS-3363

Upstream issues: https://github.com/quarkusio/quarkus/issues/13522, https://github.com/quarkusio/quarkus/issues/30002

Upstream PRs: https://github.com/quarkusio/quarkus/pull/49408, https://github.com/quarkusio/quarkus/pull/43764

Quarkus 3.26 introduced support for Hibernate ORM offline startup.
When the offline startup is enabled, Hibernate skips connecting to the database during application startup.
By default, the Hibernate ORM fetches database metadata at startup and fails if the database is unreachable.
With an offline startup, Hibernate ORM metadata required on startup is configured with suitable defaults based on the database kind.
Quarkus documentation mentions Kubernetes deployment as one of the use cases for this feature.
Because JDBC metadata varies for each database, it is essential to test all supported databases.
Flyway extension is mentioned in the documentation as one option for database schema migration;
however, this extension is not supported with the product and may not be suitable for all databases and scenarios we want to test, therefore I consider testing it optional.

## Scope of the testing

Test coverage should verify:

* multitenant scenario when JDBC credentials are provided by the user on the first request (covers our customer scenario)
* feature works in JVM mode, DEV mode, and native mode
* feature works on bare-metal with supported databases DB2, PostgreSQL, MariaDB, MySQL, Oracle, and SQL Server
* feature works in OpenShift with PostgreSQL, MySQL, and MariaDB; testing DB2, SQL Server, and Oracle is out of scope, considering we don’t have any such tests running
* smoke-check for dialect fine-tuning
* smoke-check for Hibernate Reactive offline startup (currently a tech-preview) with PostgreSQL
* smoke-check for OpenTelemetry JDBC instrumentation

## Existing test coverage

Quarkus QE has no test coverage.
Upstream test coverage extensively checks that JDBC metadata is configured correctly for H2, MySQL, MariaDB, PostgreSQL, and SQL Server;
however, I didn’t find a single scenario that actually uses the database.
All the tests seem to open a Hibernate Session and check its properties.

### Impact on test suites and testing automation

A new module `hibernate/hibernate-offline-startup` will be added to our Quarkus QE Test Suite.
The `sql-db-modules` Maven profile is already the longest-running profile, and we have to [manually divide](https://github.com/quarkus-qe/quarkus-test-suite/blob/cea391cad1046bfa91e832a99507b9c5262d3c8a/.github/workflows/ci.yml#L92) it when we want to execute its tests.
Therefore, a new Maven profile `hibernate-modules` will be introduced, and we will move there also `sql-db/hibernate`, `sql-db/hibernate-reactive`, `sql-db/hibernate-fulltext-search`, and `sql-db/jakarta-data`.

### Impact on resources

The test execution time will increase by more than one hour.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Hibernate ORM - Offline startup](https://quarkus.io/guides/hibernate-orm#offline)
- [Add hibernate.boot.allow_jdbc_metadata_access](https://hibernate.atlassian.net/browse/HHH-17269)
- [Draft PR - QUARKUS-3363 - Offline startup for Hibernate ORM in Quarkus](https://github.com/quarkusio/quarkus/pull/43396)
- [Infer database capabilities from JDBC-provided metadata on runtime init](https://github.com/quarkusio/quarkus/pull/13435)
- [More control over when hibernate init and finish the connections in the app](https://github.com/quarkusio/quarkus/issues/29846)
- [Migration Guide 3.26 - MySQL/MariaDB storage engine](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.26#mysqlmariadb-storage-engine)
