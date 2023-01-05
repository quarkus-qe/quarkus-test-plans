# QUARKUS-2788 Support setting the RolesAllowed in the Panache REST Data extension

JIRA links:
- https://issues.redhat.com/browse/QUARKUS-2788 - Support setting the RolesAllowed in the Panache REST Data extension
- https://issues.redhat.com/browse/QUARKUS-2786 - Propagate the javax.annotation.security annotations in REST Data

Quarkus documentation:
- https://quarkus.io/version/main/guides/rest-data-panache
- https://quarkus.io/version/main/guides/security-overview-concept
- https://quarkus.io/version/main/guides/spring-data-rest

Related test plans:
- https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-2390.md
- https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-2388.md

This test plan covers 2 JIRA tickets which are closely related (see JIRA links above).

## Scope of testing
Both topics deal with integration of REST Data Panache and annotation-based security supported by Quarkus and provided
by annotations within the package `javax.annotation.security`:
- QUARKUS-2786 enables propagation of the security annotations defined on Panache resource 
  (`PanacheEntityResource`, `PanacheRepositoryResource`) into generated JAX-RS resources.
  - This works for extensions built on top of `quarkus-rest-data-panache`:
    - `quarkus-hibernate-orm-rest-data-panache`
    - `quarkus-hibernate-reactive-rest-data-panache` (experimental)
    - `quarkus-mongodb-rest-data-panache` (experimental)
    - `quarkus-spring-data-rest` - the feature is not documented in the extension's guide
- QUARKUS-2788 adds `rolesAllowed` attribute into the `@ResourceProperties` and `@MethodProperties` annotations
  (provided by `quarkus-rest-data-panache`), which can serve as an alternative way of specifying access permission
  to the resources.
  - This also works for all extensions built on top of `quarkus-rest-data-panache` except `quarkus-spring-data-rest`,
    since the annotations are not applicable to Spring repositories.

### Existing tests
No existing test coverage which would actually test resource access has been found.
The upstream integration tests only verify security scheme via OpenAPI integration.

## Automated test development
Add a set of secured resources + security scenario + OpenShift security scenario into existing modules in Quarkus QE TS:
- `quarkus-hibernate-orm-rest-data-panache` - module `sql-db/panache-flyway`
- `quarkus-spring-data-rest` - module `spring/spring-data`

The set of resources and their related tests could be inspired by the security coverage in module `security/basic`.
It should in any case cover all positive and negative test cases that arise from usage of the security annotations:
- `@RolesAllowed`
- `@DenyAll`
- `@PermitAll`
- `io.quarkus.rest.data.panache.ResourceProperties#rolesAllowed` (not applicable to `quarkus-spring-data-rest`)
- `io.quarkus.rest.data.panache.MethodProperties#rolesAllowed` (not applicable to `quarkus-spring-data-rest`)

## Advanced topics for test development
Coverage for experimental extensions - for the future, not within the scope of this plan:
- `quarkus-hibernate-reactive-rest-data-panache`
- `quarkus-mongodb-rest-data-panache`

## Impact on test suites and test environment
2 new scenarios (in 2 modules), each one will increase the execution time by circa:
2 / 5 / 4 / 8 minutes in JVM / native / OpenShift JVM / OpenShift native mode.

No other impacts are expected.
