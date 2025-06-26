# QUARKUS-5663 Support for OIDC MTLS binding

JIRA: https://issues.redhat.com/browse/QUARKUS-5663

Support for OIDC MTLS binding

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/45121

Upstream issue:
- https://github.com/quarkusio/quarkus/issues/4482

Documentation:
- https://quarkus.io/guides/security-oidc-bearer-token-authentication#mutual-tls-token-binding

## Scope of testing
This feature added mechanism for binding access tokens to Mutual TLS client authentication certificates.
That mean that the JWT token checking the `x5t#S256` (X.509 certificate SHA-256 thumbprint) claim against the presented client certificate during mTLS handshake.
The tests will check that token contains the `x5t#S256` and it is/isn't able to authenticate.

## Automated test development
The tests will check a JWT token containing the correct `x5t#S256` claim can successfully authenticate and access the protected endpoint.
An otherwise valid JWT token with a modified and absent `x5t#S256` claim will be rejected, and authentication will fail.

The mTLS connection will be established between the Keycloak identity provider and the Quarkus client application.

### Impact on TS and resources
These tests will be added to the `security/oidc-client-mutual-tls` module and executed in both JVM and native modes on bare metal and OCP.
The expected time increase should be in number of minutes, when the Quarkus app need to be re-build.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
