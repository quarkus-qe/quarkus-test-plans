# QUARKUS-2388 Promote Spring Data REST from TP to full support

JIRA link: https://issues.redhat.com/browse/QUARKUS-2388

Quarkus documentation: https://quarkus.io/version/main/guides/spring-data-rest

Tech preview test plan: [QUARKUS-714](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-714.md)

Spring Data REST compatibility layer provides a subset of Spring Data REST features.
The existing Quarkus extension (`spring-data-rest`) remains unchanged.

## Scope of testing

### Existing tests

[Quarkus upstream unit tests](https://github.com/quarkusio/quarkus/tree/main/extensions/spring-data-rest/deployment/src/test)
- CrudRepository
- PagingAndSortingRepository
- JpaRepository
- @RepositoryRestResource
- Use of both supported data types: `application/json`, `application/hal+json`

[Quarkus upstream integration tests](https://github.com/quarkusio/quarkus/tree/main/integration-tests/spring-data-rest)
- CrudRepository
- PagingAndSortingRepository
- Use of both supported data types: `application/json`, `application/hal+json`

[Quarkus Quickstarts](https://github.com/quarkusio/quarkus-quickstarts/tree/main/spring-data-rest-quickstart)
- PagingAndSortingRepository

[Quarkus QE test suite](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/spring/spring-data)
- CrudRepository
- PagingAndSortingRepository
- @RepositoryRestResource
- Use of just one data type: `application/json`
- Native and OpenShift coverage
- Special cases: invoking disallowed method

## Automated test development
Quarkus QE test suite - module `spring/spring-data`:
- Add JpaRepository test scenario.
- Include alternative data type (`application/hal+json`) tests in the scenario.
- Extend special cases: get by invalid ID, save and update invalid entity, delete by invalid ID.

## Impact on test suites and test environment
No new environment requirements.
Impact of 1 new scenario - time increase: 2 / 5 / 4 / 8 minutes in JVM / native / OpenShift JVM / OpenShift native mode.
