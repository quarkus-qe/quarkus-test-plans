# QUARKUS-5655 Annotation to allow using custom CDI bean methods as permission checkers

JIRA: https://issues.redhat.com/browse/QUARKUS-5655

Upstream PRs:
- https://github.com/quarkusio/quarkus-security/pull/56
- https://github.com/quarkusio/quarkus/pull/43846

## Scope of testing
- Cover new annotation `@PermissionChecker` which allows custom code to do authorization checks for specific permission.
- Test cases will cover unauthorized access, permission denied as well as allowed cases.
- Inclusive authorization (requiring multiple permissions simultaneously) will be covered.
- Tests will cover having `@Blocking` as well as non-blocking version of permissionCheckers.

## Getting familiar with the feature
Get familiar with
- https://quarkus.io/guides/security-authorize-web-endpoints-reference#permission-checker

## Existing test coverage
QE TS already covers cases for authentication and authorization.
These will be expanded to cover also permissionChecker annotation.

### Impact on test suite
Module security/keycloak-oidc-client-reactive-extended will be expanded with new test cases.
New tests will be added into `OidcRestClientIT` class.

## Impact on resources
No new test class will be created. Additional execution time should be negligible.

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>