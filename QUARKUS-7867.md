# QUARKUS-7867 - Support for OIDC SPIFFE Client Authentication

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7867

Upstream issue: https://github.com/quarkusio/quarkus/issues/52232

Upstream PR: https://github.com/quarkusio/quarkus/pull/53773

This feature adds support for SPIFFE JWT-SVID client authentication in Quarkus OIDC and OIDC Client.
When configured, Quarkus reads a SPIFFE token from the filesystem and uses it as a client assertion to authenticate to the OIDC provider.
It builds on the existing JWT bearer authentication mechanism with a new `spiffe` source option.

## Scope of the testing

Verify that SPIFFE JWT-SVID tokens are correctly used for client authentication against a Keycloak instance with federated client authentication enabled.
Testing will use the existing Keycloak infrastructure in `security/keycloak-oidc-client-extended`.

Tests will cover:

* A SPIFFE JWT-SVID token loaded from the filesystem is used as the `client_assertion` when authenticating to Keycloak via the client credentials grant.
* A SPIFFE JWT-SVID token is used for client authentication during the authorization code flow, and the user completes the flow successfully.
* When the SPIFFE token on disk expires and is replaced with a new token, the application re-reads the file and authenticates with the new token.
* A token with a `sub` claim that does not start with `spiffe://` is rejected and authentication fails.
* A token with a missing expiration claim is rejected and authentication fails.
* A token with a missing audience claim is rejected by Keycloak and authentication fails.

## Existing test coverage

Upstream unit tests: https://github.com/quarkusio/quarkus/blob/main/extensions/oidc-common/runtime/src/test/java/io/quarkus/oidc/common/runtime/providers/KubernetesServiceClientAssertionProviderTest.java

Upstream integration tests: https://github.com/quarkusio/quarkus/tree/main/integration-tests/oidc-client-wiremock and https://github.com/quarkusio/quarkus/tree/main/integration-tests/oidc-wiremock

Upstream unit tests cover SPIFFE token refresh, invalid `sub` claim rejection, and correct assertion type values.
Upstream integration tests verify the correct `client_assertion_type` and `client_assertion` parameters are sent during client credentials and code flow authentication, using WireMock stubs.
Upstream does not test against a real Keycloak instance or on OpenShift.

The QE test suite has existing JWT bearer filesystem authentication tests in `BearerFilesystemIT` within `security/keycloak-oidc-client-extended`, but does not contain any SPIFFE-specific tests.

### Impact on test suites and testing automation

New SPIFFE tests will be added to `security/keycloak-oidc-client-extended`.
The module already has Keycloak infrastructure, JWT bearer filesystem tests, and OIDC client patterns that SPIFFE tests can follow.
Keycloak will need federated client authentication enabled and a client configured to accept SPIFFE JWT-SVID tokens.
The Keycloak realm configuration will need to be updated to support SPIFFE trust domain validation.

### Impact on resources

Tests will be executed on baremetal and OpenShift in JVM and native mode.
The new tests require an additional Keycloak container with SPIFFE features enabled.
The estimated execution time increases by a few minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Reviewed upstream issue, pull request, and test coverage
- Reviewed Keycloak federated client authentication and SPIFFE client authentication documentation
- Focus on exploratory testing of the feature

## Contacts

* Tester: Slimane Abzar <sabzar@redhat.com>

## References

- https://quarkus.io/guides/security-oidc-code-flow-authentication
- https://datatracker.ietf.org/doc/draft-schwenkschuster-oauth-spiffe-client-auth/
- https://www.keycloak.org/2026/01/federated-client-authentication
- https://github.com/sabre1041/keycloakcon-spiffe/
