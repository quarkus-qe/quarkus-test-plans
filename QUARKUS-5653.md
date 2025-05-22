# QUARKUS-5653 Support OidcProviderClient injection and token revocation

JIRA: https://issues.redhat.com/browse/QUARKUS-5653

Upstream PR: https://github.com/quarkusio/quarkus/pull/44993

## Scope of testing
Verify the behavior of the newly injectable and SecurityIdentity-accessible `OidcProviderClient` component:
- Security event-driven token revocation
- Programmatic token revocation using injected `OidcProviderClient`
- Token introspection and `UserInfo` features behavior verification

## Getting familiar with the feature
Get familiar with examples in the guide:
- https://quarkus.io/guides/security-oidc-code-flow-authentication#oidc-token-revocation

## Existing test coverage in upstream
- Inspecting access tokens using injected beans

[CodeFlowTest#testAccessTokenInjection()](https://github.com/quarkusio/quarkus/blob/main/integration-tests/oidc-code-flow/src/test/java/io/quarkus/it/keycloak/CodeFlowTest.java#L1153)

- Firing security event and revoke tokens on logout

[CodeFlowAuthorizationTest](https://github.com/quarkusio/quarkus/blob/main/integration-tests/oidc-wiremock-logout/src/test/java/io/quarkus/it/keycloak/CodeFlowAuthorizationTest.java)

## Impact on test suites and testing automation
New test class will be added to [security/keycloak](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security/keycloak) module in QQE TS.

## Impact on resources
The expected execution time will increase in about 1.5 minutes for JVM mode and 4 minutes for native.

## Contacts
* Tester: Georgii Troitskii <gtroitsk@redhat.com>