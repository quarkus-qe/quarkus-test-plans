# QUARKUS-1408 - Dev Service for MS SQL Client

JIRA link: https://issues.redhat.com/browse/QUARKUS-1408

Related test plan: [Quarkus-959 - Improved Developer Joy with DevServices](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-959.md)  
Related test development: [Reactive PostgreSQL and MySQL Dev Services ](https://issues.redhat.com/browse/QUARKUS-1080)  

Relevant Quarkus community documentation:
- https://quarkus.io/version/main/guides/datasource#reactive-datasource-2
- https://quarkus.io/version/main/guides/datasource#dev-services
- https://quarkus.io/version/main/guides/reactive-sql-clients

Reactive MS SQL client has been added to Quarkus in https://github.com/quarkusio/quarkus/pull/18311.
Dev Services for MS SQL client are present in Quarkus since https://issues.redhat.com/browse/QUARKUS-959 (see related test plan).

This is an integration of the reactive MS SQL client with Dev Services.

## Scope of testing

### Existing test coverage
Test in Quarkus upstream:
- https://github.com/quarkusio/quarkus/tree/main/extensions/reactive-mssql-client/deployment/src/test/java/io/quarkus/reactive/mssql/client

## Automated test development
In QE TS, extend the `sql-db/reactive-vanilla` with `mssql` datasource and separate MS SQL scenario:
- Test that datasource is operational.
- Test that Dev Services correctly initialize the MS SQL container.

## Impact on test suites and test environment
1 new scenario in existing module, additional pull and start of MS SQL image.
Estimated time increase:
- 1 minute for JVM mode
- 5 minutes for native mode
