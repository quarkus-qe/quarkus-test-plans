# QUARKUS-5660 Allow to create static OIDC tenants programmatically

JIRA: https://issues.redhat.com/browse/QUARKUS-5660

Upstream PR:
- https://github.com/quarkusio/quarkus/pull/45294

## Scope of testing
- Test programmatically creation and configure static OIDC tenants using `OidcTenantConfig` builder
- Test new `Oidc` interface methods - cover creation of custom OIDC tenants and default tenants with service or web application types

## Getting familiar with the feature
Get familiar with
- https://quarkus.io/version/main/guides/security-oidc-bearer-token-authentication#programmatic-oidc-start-up
- https://quarkus.io/version/main/guides/security-oidc-code-flow-authentication#programmatic-oidc-start-up
- https://quarkus.io/version/main/guides/security-openid-connect-multitenancy#programmatic-startup

## Existing test coverage
Creating custom tenants using `OidcTenantConfig` builder with multiple OIDC tenant properties
- https://github.com/quarkusio/quarkus/blob/main/extensions/oidc/deployment/src/test/java/io/quarkus/oidc/test/UserInfoRequiredDetectionTest.java
Creating default tenants for different application types
- https://github.com/quarkusio/quarkus/blob/main/extensions/oidc/runtime/src/test/java/io/quarkus/oidc/runtime/OidcImplTest.java

## Impact on resources
Upstream coverage is sufficient, no new tests in QE test suite will be created.

## Contacts
* Tester: Georgii Troitskii <gtroitsk@redhat.com>