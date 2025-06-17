# QUARKUS-5653 Support OidcProviderClient injection and token revocation

JIRA: https://issues.redhat.com/browse/QUARKUS-5653

Upstream PR: https://github.com/quarkusio/quarkus/pull/44993

## Scope of testing
Verify the behavior of the newly injectable and SecurityIdentity-accessible `OidcProviderClient` component:
- Programmatic token revocation using injected `OidcProviderClient`
- Token introspection and `UserInfo` features behavior verification
- (Not covered) Security event-driven token revocation using `SecurityEventListener` due to current limitations with Keycloak (see below)

## Getting familiar with the feature
Get familiar with examples in the guide:
- https://quarkus.io/guides/security-oidc-code-flow-authentication#oidc-token-revocation

## Existing test coverage in upstream
- Inspecting access tokens using injected beans

[CodeFlowTest#testAccessTokenInjection()](https://github.com/quarkusio/quarkus/blob/main/integration-tests/oidc-code-flow/src/test/java/io/quarkus/it/keycloak/CodeFlowTest.java#L1153)

- Firing security event and revoke tokens on logout

[CodeFlowAuthorizationTest](https://github.com/quarkusio/quarkus/blob/main/integration-tests/oidc-wiremock-logout/src/test/java/io/quarkus/it/keycloak/CodeFlowAuthorizationTest.java)

## Why the feature cannot be fully tested
While programmatic token revocation using the injected `OidcProviderClient` is testable,
we cannot test event-driven token revocation via `SecurityEventListener` with Keycloak due to the following limitations:
- Keycloak automatically revokes tokens during logout, e.g., on session close. This makes it impossible to distinguish between:
  - Internal revocation by Keycloak
  - Our own revocation logic triggered in the event listener

Because of this, we cannot reliably assert that our custom revocation was executed, nor confirm its effect independently.
The only way to test event-driven revocation is via mocking, which is already done in the upstream test.

## Impact on test suites and testing automation
New tests will be added to [security/keycloak](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security/keycloak) module in QQE TS.

## Impact on resources
The expected execution time will increase in the range of seconds in JVM and Native mode for bare metal and OpenShift.

## Contacts
* Tester: Georgii Troitskii <gtroitsk@redhat.com>