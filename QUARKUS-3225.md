# QUARKUS-3225 - Test and Verify RHBQ 3.8 on RHEL using a FIPS enabled system and OpenJDK

JIRA: https://issues.redhat.com/browse/QUARKUS-3225

PRs tracked in: https://issues.redhat.com/browse/QQE-528

This feature verifies that Red Hat build of Quarkus functions correctly when run in FIPS-enabled environment.
More specifically, we aim to support application run on RHEL with supported OpenJDK versions.
We only verify the Quarkus application itself, not integrated systems running in FIPS-enabled environments.
Nevertheless, considering we run containers in FIPS-enabled environment as well, we test integrated systems (like databases) with proper FIPS setup as well.

## Scope of the testing
Run the Quarkus QE Test Suite baremetal scenarios in FIPS-enabled environment on RHEL with supported OpenJDK versions.
We want to test native mode with Mandrel distribution, JVM mode and DEV mode as part of the extended platform trigger.
All the tests will be run with Docker, Podman is currently out of scope. Mostly because here, we care only about Quarkus application itself.
The test coverage should be exactly same as for baremetal scenarios in FIPS-disabled environment.
Should any of tests fail in FIPS-enabled environment, we will disable the test, report upstream issue, 
create product tracker and document the fact in the product supported configurations.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
We run Quarkus QE Test Suite in JVM mode with OpenJDK 17 and 21, in the DEV mode and in the native mode.
Prior to this effort, test coverage has the following differences to the FIPS-disabled environment:

- 49 disabled tests
- over 20 more test failed even though not tagged as FIPS-incompatible, including:
  - Kafka SSL/SASL
  - all SQL server scenarios with OpenJDK 17
  - Management interface TLS scenarios
  - WebAuth tests using MySQL

### Impact on test suites and testing automation
The FIPS-enabled and FIPS-disabled environments baremetal tests coverage run with OpenJDK supported versions will only differ in:

- WebAuth tests coverage is disabled as the test uses MySQL Reactive client
  - upstream issue: https://github.com/eclipse-vertx/vertx-sql-client/issues/1436
  - product tickets: https://issues.redhat.com/browse/QUARKUS-4387, https://issues.redhat.com/browse/QUARKUS-4332
  - disabled tests: https://github.com/quarkus-qe/quarkus-test-suite/blob/main/security/webauthn/src/test/java/io/quarkus/ts/security/webauthn/MySqlWebAuthnIT.java
- Hibernate Reactive MySQL tests and direct use of MySQL Reactive client are disabled due to the same issue as WebAuth (see above)
  - disabled tests: 
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/hibernate-reactive/src/test/java/io/quarkus/ts/hibernate/reactive/MySQLDatabaseHibernateReactiveIT.java
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/reactive-rest-data-panache/src/test/java/io/quarkus/ts/reactive/rest/data/panache/MySqlPanacheResourceIT.java
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/vertx-sql/src/test/java/io/quarkus/ts/vertx/sql/handlers/MysqlHandlerIT.java
- MySQL DEV Mode tests are disabled as TestContainers are using older driver that uses an authentication mechanism with weaker cipher.
  This issue is likely to fix itself as TestContainers will bump their dependencies.
  - upstream issue: https://github.com/quarkusio/quarkus/issues/40526
  - disabled tests:
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/reactive-vanilla/src/test/java/io/quarkus/ts/reactive/db/clients/DevModeReactiveMysqlDevServiceUserExperienceIT.java
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/sql-app/src/test/java/io/quarkus/ts/sqldb/sqlapp/DevModeMysqlDevServiceUserExperienceIT.java
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/sql-app/src/test/java/io/quarkus/ts/sqldb/sqlapp/DevModeMysqlIT.java
- DB2 tests are disabled due to known issue. I attempted to fix it with encrypted configuration and certs generated according to the docs.
  Results were absolutely same. I also tried JWT-based authentication, but then I run into problems with both DB2 and Hibernate (that expects username / password).
  - known issue: https://github.com/IBM/Db2/issues/43
  - Quarkus SMEs feedback: currently FIPS support is out of the scope
  - disabled tests:
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/sql-app/src/test/java/io/quarkus/ts/sqldb/sqlapp/DB2DatabaseIT.java
    - https://github.com/quarkus-qe/quarkus-test-suite/blob/main/sql-db/sql-app-compatibility/src/test/java/io/quarkus/ts/sqldb/compatibility/DB2DatabaseIT.java
- SQL Server scenarios are only going to be run with OpenJDK 21+ and tests with both encrypted communication (recommended) and unsecured communication.
  - upstream issue: https://github.com/quarkusio/quarkus/issues/40813
  - product ticket: https://issues.redhat.com/browse/QUARKUS-4330
  - 8 disabled tests for OpenJDK 17: https://github.com/quarkus-qe/quarkus-test-suite/pull/1807

Fixed product tickets:

- https://issues.redhat.com/browse/QUARKUS-4331
- https://issues.redhat.com/browse/QUARKUS-4329
- https://issues.redhat.com/browse/QUARKUS-2036

### Impact on resources
Extended test execution that will basically match FIPS-disabled baremetal test execution times.
Considering we enabled over 40 tests (I merged some tests so numbers may not add up), that's over 90 minutes of additional time. 

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [QUARKUS-1159 - Ensure Quarkus runs on a FIPS enabled RHEL 8 System](https://issues.redhat.com/browse/QUARKUS-1159)
- [QUARKUS-1159 - Test Plan](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-1159.md)