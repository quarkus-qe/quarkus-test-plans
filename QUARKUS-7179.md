# QUARKUS-7179 - Support `@PermissionsAllowed` security annotation on REST Data Panache endpoints

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7179

Upstream issue: https://github.com/quarkusio/quarkus/issues/38966

Upstream PR: https://github.com/quarkusio/quarkus/pull/51188

REST Data with Panache auto-generates REST endpoints from Panache resource interfaces.
Security annotations like `@RolesAllowed` and `@DenyAll` were supported on these interfaces, but `@PermissionsAllowed` was not.
This feature adds support for `@PermissionsAllowed` so that it is propagated to the generated endpoints, allowing permission-based access control.

## Scope of the testing

Verify that `@PermissionsAllowed` is correctly enforced on generated REST Data Panache endpoints.
Testing will cover both `quarkus-hibernate-orm-rest-data-panache` and `quarkus-hibernate-reactive-rest-data-panache` with PostgreSQL.

Tests will cover:

* `@PermissionsAllowed` placed on the resource interface (class-level) secures all generated endpoints including list, get, add, update and delete.
* `@PermissionsAllowed` placed on a specific method secures only that endpoint while other generated endpoints remain accessible.
* Repeated `@PermissionsAllowed` annotations on a method require all permissions to be satisfied.
* Method-level annotations override class-level annotations when both are present (e.g. class-level `@RolesAllowed` with method-level `@PermissionsAllowed`).
* The annotation works on both `PanacheEntityResource` and `PanacheRepositoryResource` interfaces.
* `@PermissionsAllowed` on a custom user-defined method behaves consistently with the generated endpoints.
* Users with the required permission receive successful responses (200/204).
* Users without the required permission receive HTTP 403, including users with partial permissions when multiple are required (e.g. user has `foo-1` but not `foo-2`).
* Unauthenticated users and users with invalid credentials receive HTTP 401.

Permission evaluation will use `@PermissionChecker` methods to check permissions based on the user's roles.

## Existing test coverage

Upstream tests: https://github.com/quarkusio/quarkus/tree/main/extensions/panache/hibernate-orm-rest-data-panache/deployment/src/test/java/io/quarkus/hibernate/orm/rest/data/panache/deployment/security

Upstream test coverage verifies `@PermissionsAllowed` at method-level and class-level, repeated annotations, and combinations with `@RolesAllowed`.
All upstream tests use H2 in-memory databases and embedded test identity providers.
It does not test with a real database or on OpenShift.

The QE test suite does not contain any tests for `@PermissionsAllowed` on REST Data Panache endpoints.

### Impact on test suites and testing automation

The test suite modules will be restructured:

* `sql-db/reactive-rest-data-panache` will be renamed to `sql-db/hibernate-orm-rest-data-panache`.
* Existing QUARKUS-2788 REST Data Panache security coverage will be moved from `sql-db/panache-flyway` to `sql-db/hibernate-orm-rest-data-panache`.
* A new `sql-db/hibernate-reactive-rest-data-panache` module will be created for reactive coverage.

New `@PermissionsAllowed` blocking tests will be added to `sql-db/hibernate-orm-rest-data-panache`.
New `@PermissionsAllowed` reactive tests will be added to `sql-db/hibernate-reactive-rest-data-panache`.

### Impact on resources

Tests will be executed on baremetal and OpenShift in JVM and native mode.
The new tests reuse the existing module infrastructure, so the impact should be minimal.
The estimated execution time increases by a few minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Reviewed upstream issue, pull request and test coverage
- Reviewed Quarkus REST Data with Panache documentation on security annotations
- Focus on exploratory testing of the feature

## Contacts

* Tester: Slimane Abzar <sabzar@redhat.com>

## References

- https://quarkus.io/guides/rest-data-panache
