# QUARKUS-5858 Tech preview Support for OAuth2 Demonstrating Proof of Possession

JIRA: https://issues.redhat.com/browse/QUARKUS-5858

Upstream PR: https://github.com/quarkusio/quarkus/pull/45891

Upstream documentation:
* https://quarkus.io/version/main/guides/security-oidc-bearer-token-authentication#demonstrating-proof-of-possession-dpop

## Scope of testing
Verify Quarkus works with authentication mechanism "Demonstrating Proof of Possession" (DPoP).
A new test class will be implemented in `security/keycloak-oidc-client-extended`.
Tests will verify authentication using DPoP, which will cover test cases:
* Happy path - correctly accessing resource using DPoP authentication.
* Wrong authentication method - try using just JWT token, while DPoP is enforced. 
* DPoP Authentication failures:
  * No DPoP header.
  * DPoP proof signed with wrong key.
  * Mismatching HTTP method in DPoP proof.
  * Mismatching HTTP path in DPoP proof.

### Impact on resources
A new module will be created, requires additional test execution.
Tests will be executed on both JVM and native and on both baremetal and OCP.
Expected additional execution time in minutes for each case.

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>