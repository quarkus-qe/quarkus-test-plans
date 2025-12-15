# QUARKUS-6971 - Support method-level @OidcClientFilter bindings

JIRA: https://issues.redhat.com/browse/QUARKUS-6971

Upstream issue: https://github.com/quarkusio/quarkus/issues/50802
Upstream PR: https://github.com/quarkusio/quarkus/pull/50806
Documentation: https://quarkus.io/guides/security-openid-connect-client-reference#rest-client-oidc-filter

## Scope of testing
This feature extends existing feature of adding Oidc access tokens into quarkus REST clients using `@OidcClientFilter` annotation.
Prior to this feature, it has to be set on entire REST client.
Now it is more granular and can be set either on entire REST client, or only on specific methods,
allowing to inject tokens only for specific requests.

Test cases:
* Methods with `@OidcClientFilter` get an access token injected.
* Methods without `@OidcClientFilter` doesn't get the token injected.
* Methods with named `@OidcClientFilter` get token from correct OIDC client.

## Existing test coverage
QE TS has several tests covering `@OidcClientFilter` functionality on entire REST client interface.

## Impact on test suite
New tests scenarios will be added into modules `security/keycloak-oidc-client-extended` and `security/keycloak-oidc-client-reactive-extended`.
Test cases will be added into existing test class `AbstractOidcRestClientIT`

## Impact on resources
New tests will not require an additional app build.
Impact on execution time should be negligible.

## Contacts
* Tester: Martin Ocenas <mocenas@ibm.com>