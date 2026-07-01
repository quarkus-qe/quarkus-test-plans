# QUARKUS-7340 - OIDC: Support for custom DPoP nonce providers

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7340

Upstream PR: https://github.com/quarkusio/quarkus/pull/52090

This PR adds support for custom DPoP nonce providers in Quarkus OIDC by introducing the `DPoPNonceProvider` interface. Applications can implement this interface to provide and validate DPoP nonces during authentication.

## Scope of the testing

Tests will cover basic custom DPoP nonce provider behaviour for Quarkus OIDC protected resources.

* A DPoP request with a valid nonce is accepted successfully.
* A DPoP request without a nonce returns `401 Unauthorized` with the expected DPoP challenge headers (`WWW-Authenticate`, `DPoP-Nonce` and `Cache-control: no-store`).
* A DPoP request with an invalid nonce returns `401 Unauthorized` with the expected DPoP challenge headers.
* A client can successfully retry the request using the nonce returned in the initial challenge.

Note: The `WWW-Authenticate` header will be checked for the invalid nonce scenario to ensure it contains `use_dpop_nonce` and does **not** contain `invalid_dpop_proof`, providing regression coverage for https://github.com/quarkusio/quarkus/issues/54854.

## Existing test coverage

Existing upstream tests are located here: https://github.com/quarkusio/quarkus/tree/main/integration-tests/oidc-dpop

Upstream test coverage includes tests for the custom `DPoPNonceProvider` flow, including scenarios for missing nonce, invalid nonce and successful retry with a valid nonce.

The QE test suite contains DPoP coverage in `security/keycloak-oidc-client-extended` however it does **not** currently cover DPoP with a custom nonce provider.

### Impact on test suites and testing automation

* The new tests will extend the existing security coverage in the QE test suite under `security/keycloak-oidc-client-extended`. 
* A dedicated custom DPoP nonce provider scenario will be added and existing DPoP tests will remain unchanged.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should only be a few minutes.
* Tests will be executed on baremetal and OpenShift in JVM and native mode. 

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream issue and pull request.
* Reviewed documentation on DPoP.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/security-oidc-bearer-token-authentication#demonstrating-proof-of-possession-dpop
* https://datatracker.ietf.org/doc/html/rfc9449