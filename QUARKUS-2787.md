# QUARKUS-2787 Rest Data Panache: Correct Open API integration

JIRA link: https://issues.redhat.com/browse/QUARKUS-2787

Quarkus documentation:
- https://quarkus.io/version/main/guides/rest-data-panache
- https://quarkus.io/version/main/guides/openapi-swaggerui
- https://quarkus.io/version/main/guides/spring-data-rest

Related test plans:
- https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-2390.md
- https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-2388.md

This test plan covers the OpenAPI integration with Quarkus extensions based on `quarkus-rest-data-panache`.

## Scope of testing
The OpenAPI functionality was limited - see linked JIRA. The aim is to develop test for basic OpenAPI functionality,
including the features described in the JIRA.
Affected extensions:
- `quarkus-hibernate-orm-rest-data-panache`
- `quarkus-hibernate-reactive-rest-data-panache` (experimental)
- `quarkus-mongodb-rest-data-panache` (experimental)
- `quarkus-spring-data-rest` - the REST Data Panache feature is not documented in the extension's guide

### Existing tests
There are integration tests in upstream for:
- `quarkus-hibernate-orm-rest-data-panache` - https://github.com/quarkusio/quarkus/blob/main/extensions/panache/hibernate-orm-rest-data-panache/deployment/src/test/java/io/quarkus/hibernate/orm/rest/data/panache/deployment/openapi/OpenApiIntegrationTest.java
- `quarkus-hibernate-reactive-rest-data-panache` - https://github.com/quarkusio/quarkus/blob/main/extensions/panache/hibernate-reactive-rest-data-panache/deployment/src/test/java/io/quarkus/hibernate/reactive/rest/data/panache/deployment/openapi/OpenApiIntegrationTest.java

They provide sufficient coverage of the OpenAPI functionality for the extensions.

## Automated test development
Adopt the upstream integration tests for extensions not yet covered:
- `quarkus-spring-data-rest` - module `spring/spring-data`

## Advanced topics for test development
- `quarkus-mongodb-rest-data-panache` - for the future, this extension is not supported on product side yet.

## Impact on test suites and test environment
1 new scenario, expected increase in the execution time:
2 / 5 / 4 / 8 minutes in JVM / native / OpenShift JVM / OpenShift native mode.

No other impacts are expected.
