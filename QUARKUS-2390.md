# QUARKUS-2390 Promote quarkus-hibernate-orm-rest-data-panache extension from TP to full support

JIRA link: https://issues.redhat.com/browse/QUARKUS-2390

Quarkus documentation: https://quarkus.io/version/main/guides/rest-data-panache

Topic previously covered in test plan [QUARKUS-976](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-976.md)

REST resources for Hibernate ORM with Panache (`hibernate-orm-rest-data-panache`) provides basic CRUD endpoints
(JAX-RS resources) for entities and repositories. It does so by generating implementation to all user-defined
`PanacheEntityResource` / `PanacheRepositoryResource` interfaces, which correspond with `PanacheEntity` / `PanacheRepository`
definitions. These implementations are then injected into generated JAX-RS resources.

There is also a way of customising the generated JAX-RS resources by using the `@ResourceProperties` and `@MethodProperties`
annotations in the resource interfaces.

The extension offers compatibility with Hibernate ORM (non-reactive) and with both RESTEasy Classic and RESTEasy Reactive extensions.

## Scope of testing

### Existing tests

[Quarkus upstream unit tests](https://github.com/quarkusio/quarkus/tree/main/extensions/panache/hibernate-orm-rest-data-panache/deployment/src/test)
- CRUD operations on `PanacheEntityResource` and `PanacheRepositoryResource`.
- Annotation customization using all supported annotations and their attributes.
- Use of both supported data types: `application/json`, `application/hal+json`.
- Pagination and sorting.
- Check of exposed endpoints via OpenAPI integration.

[Quarkus upstream integration tests](https://github.com/quarkusio/quarkus/tree/main/integration-tests/hibernate-orm-rest-data-panache)
- CRUD operations on `PanacheEntityResource` and `PanacheRepositoryResource`.
- Annotation customization using all supported annotations and their attributes.
- Use of both supported data types: `application/json`, `application/hal+json`.
- Sorting.
- Special cases: save and update invalid entity, get by invalid ID.

[Quarkus QE test suite](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/sql-db/panache-flyway)
- CRUD operations on `PanacheEntityResource` and `PanacheRepositoryResource`.
- Annotation customization using all supported annotations and their attributes.
- Use of both supported data types: `application/json`, `application/hal+json`.
- Pagination and sorting.
- Special cases: invoke disallowed method, save invalid entity, get by invalid ID, find by dependent property.
- Integration with RESTEasy Classic.
- Native coverage.

## Automated test development
Quarkus QE test suite
- Add a scenario which combines `hibernate-orm-rest-data-panache` with RESTEasy Reactive extension.

## Impact on test suites and test environment
No new environment requirements.
Impact of 1 new scenario - time increase: 2 / 5 / 4 / 8 minutes in JVM / native / OpenShift JVM / OpenShift native mode.
