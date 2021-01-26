# Quarkus-190 - Testing and verifying Quarkus/Vertx SQL

The goal of this test plan is to cover the main points of conflicts between Quarkus/Vertx and SQL databases. 

JIRA link: https://issues.redhat.com/browse/QUARKUS-190 


## Scope of the testing
Following actions were taken to ensure test coverage and automation for vertx/sql applications:
 - Setup databases and load initial data through Flyway
 - Programmatic insert statements
 - Select statements
 - Transactional statements between several tables
 - Tested databases: PostgreSQL, MariaDB, optionally MySQL and DB2

### Impact on testsuites and testing automation:
 - Ensure this coverage works both in JVM and NATIVE mode
 - Integration tests covered by beefy-scenarios testsuite
 - OpenShift integration tests with PosgreSQL. 

### Impact on resources:
 - MySQL (183.6 MiB - 343.5 MiB)
 - PostgreSQL (12.9 MiB)
 - DB2 (115.7Mb)

 The main impact on resources is in time. Deploy DB2 from scratch takes around 5 min.  

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features

## Automated test development

- [Quarkus/vertx - sql scenario](https://github.com/quarkus-qe/beefy-scenarios/pull/71): A flight search engine in order to test Quarkus Reactive SQL extensions.End user-facing website allows customers to compare flight prices of different airlines based on various business rules. The end phase is to book the flight in a transactional way.

## Advanced topics for test development
The following advanced topics are listed for future consideration. 
 - Improve coverage related to delete and update statements.
 - Improve db connections pool management coverage.