# QUARKUS-6551 - OIDC Client filter - allow to trigger token refresh when REST client request results in 401

JIRA: https://issues.redhat.com/browse/QUARKUS-6551

Upstream PRs:
* https://github.com/quarkusio/quarkus/pull/49003

Documentation: https://quarkus.io/version/main/guides/security-openid-connect-client-reference#rest-client-oidc-filter

## Scope of testing
New tests will be added verifying that OIDC client will automatically get a new token, if configured so, using:
* Configuration property `quarkus.rest-client-oidc-filter.refresh-on-unauthorized=true`
  * Also with named client's configuration property.
* Configured in code using `AbstractOidcClientRequestReactiveFilter::refreshOnUnauthorized()`
* If the token refresh is not configured, token will not be refreshed on the fly.

## Existing test coverage
There is an upstream test coverage this feature, but it is using devservices for keycloak.
We will test this with standalone keycloak.

### Impact on test suite
New test cases will be added into `security/keycloak-oidc-client-extended` and `security/keycloak-oidc-client-reactive-extended` modules, for both bare metal and OCP.

### Impact on resources
Test will run on bare metal and OCP, JVM and native.
It will run as part of already executed test class so impact on resources will be negligible.

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>
