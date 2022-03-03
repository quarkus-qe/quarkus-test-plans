# QUARKUS-1407 - Testing and verifying Reactive Oracle SQL client

The goal of this test plan is to cover usage of reactive Oracle DB client in Quarkus.

JIRA link: https://issues.redhat.com/browse/QUARKUS-1407 

## Related TPs
- [QUARKUS-190](QUARKUS-190.md)
- [QUARKUS-1079](QUARKUS-1079.md)
- [QUARKUS-1164](QUARKUS-1164.md)

## Scope of the testing
Following actions were taken to ensure test coverage and automation for vertx/sql applications:
 - Setup databases and load initial data through Flyway
 - Programmatic insert statements
 - Select statements
 - Transactional statements between several tables
 - Tested databases: Oracle

## Impact on testsuites and testing automation:
 - Ensure this coverage works both in JVM and NATIVE mode
 - Integration tests covered by our "main" testsuite
 - OpenShift integration (see also https://github.com/quarkus-qe/quarkus-test-suite/issues/246)

## Impact on resources:
- Oracle docker image would be used in three more modules. Startup times of `gvenzl/oracle-xe:21-slim` are around one minute. Size of the container is ~1Gb, but download is required only once

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features

## Automated test development
- Update `vertx-sql`, `hibernate-reactive` and `reactive-vanilla` test modules to use `quarkus-reactive-oracle-client`
- Add new Oracle-specific test classes, based on existing base test classes

## Advanced topics for test development
The following advanced topics are listed for future consideration. 
 - DevService
