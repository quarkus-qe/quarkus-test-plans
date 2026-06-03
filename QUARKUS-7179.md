# QUARKUS-7179 - Support `@PermissionsAllowed` security annotation on REST Data Panache endpoints

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7179

Upstream issue: https://github.com/quarkusio/quarkus/issues/38966

Upstream PR: https://github.com/quarkusio/quarkus/pull/51188

REST Data with Panache auto-generates REST endpoints from Panache resource interfaces.
Security annotations like `@RolesAllowed` and `@DenyAll` were supported on these interfaces, but `@PermissionsAllowed` was not.
This feature adds support for `@PermissionsAllowed` so that it is propagated to the generated endpoints, allowing permission-based access control.

## Scope of the testing

Verify that `@PermissionsAllowed` is correctly enforced on generated REST Data Panache endpoints.
Testing will cover the `quarkus-hibernate-orm-rest-data-panache` extension and PostgreSQL.

Tests will cover:

* Users with the required permission can access the secured endpoint and users without it receive HTTP 403. Unauthenticated users receive HTTP 401.
* `@PermissionsAllowed` placed on the resource interface (class-level) secures all generated endpoints.
* `@PermissionsAllowed` placed on a specific method secures only that endpoint.
* Repeated `@PermissionsAllowed` annotations on a method require all permissions to be satisfied.
* Method-level annotations override class-level annotations when both are present (e.g. class-level `@RolesAllowed` with method-level `@PermissionsAllowed`).
* The annotation works on both `PanacheEntityResource` and `PanacheRepositoryResource` interfaces.

## Existing test coverage

Upstream tests: https://github.com/quarkusio/quarkus/tree/main/extensions/panache/hibernate-orm-rest-data-panache/deployment/src/test/java/io/quarkus/hibernate/orm/rest/data/panache/deployment/security

Upstream test coverage verifies `@PermissionsAllowed` at method-level and class-level, repeated annotations, and combinations with `@RolesAllowed`.
All upstream tests use H2 in-memory databases and embedded test identity providers.
It does not test with a real database or on OpenShift.

The QE test suite does not contain any tests for `@PermissionsAllowed` on REST Data Panache endpoints.

### Impact on test suites and testing automation

The existing [`sql-db/panache-flyway`](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/sql-db/panache-flyway) module will be extended with new Panache resource interfaces annotated with `@PermissionsAllowed` and a `@PermissionChecker` for permission evaluation.
This module already contains REST Data Panache security tests from QUARKUS-2788.

### Impact on resources

Tests will be executed on JVM, Native and OpenShift environments.
The new tests reuse the existing module infrastructure, so the impact should be minimal.
Estimated execution time increase of a few minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Reviewed upstream issue, pull request and test coverage
- Reviewed Quarkus REST Data with Panache documentation on security annotations
- Focus on exploratory testing of the feature

## Contacts

* Tester: Slimane Abzar <sabzar@redhat.com>

## References

- https://quarkus.io/guides/rest-data-panache
