# QUARKUS-7342 - Make built-in authentication mechanism order customizable

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7342

Upstream PR: https://github.com/quarkusio/quarkus/pull/52175

This new feature allows users to configure the priority of built-in authentication mechanisms. The configured priority affects which mechanism is used first for credential verification and which mechanism sends the authentication challenge when no credentials are provided.

Supported authentication mechanism default priorities:
* Basic: 2000
* Form-based: 1000
* Mutual TLS: 1000 by default, 3000 when an inclusive authentication is enabled
* WebAuthn: 1000
* OIDC: 1001
* SmallRye JWT: 1000

## Scope of the testing
* OIDC configured with higher priority than mTLS:
    * request with both a valid OIDC bearer token and a valid mTLS certificate resolves to OIDC identity.

* mTLS configured with higher priority than OIDC:
    * request with both a valid OIDC bearer token and a valid mTLS certificate resolves to mTLS certificate identity.

* WebAuthn configured with higher priority than Basic authentication:
    * unauthenticated request receives WebAuthn login redirect instead of Basic authentication challenge.   

* Basic configured with higher priority than WebAuthn authentication:
    * unauthenticated request receives Basic authentication challenge instead of WebAuthn login redirect.

* SmallRye JWT configured with higher priority than Basic authentication:
    * unauthenticated request receives Bearer authentication challenge instead of Basic authentication challenge.

* Basic authentication configured with higher priority than SmallRye JWT:
    * unauthenticated request receives Basic authentication challenge instead of Bearer authentication challenge.

Note: Basic vs Form priority coverage is excluded, see discussion here: https://github.com/quarkusio/quarkus/issues/54948

## Existing test coverage

Existing upstream tests are located here:

* https://github.com/quarkusio/quarkus/blob/main/extensions/oidc/deployment/src/test/java/io/quarkus/oidc/test/OidcMtlsDisabledInclusiveAuthTest.java 

* https://github.com/quarkusio/quarkus/blob/main/extensions/vertx-http/deployment/src/test/java/io/quarkus/vertx/http/security/FluentApiMechanismPrioritizationTest.java

Upstream test coverage includes:
* Testing OIDC and mTLS priority, ensuring the higher priority mechanism provides the selected identity.

* Testing priority ordering when mechanisms are configured through the HTTP Security fluent API (Registers: Form, mTLS, and Basic authentication with different priorities)

The QE test suite contains coverage for individual authentication mechanisms, but no coverage for verifying and testing configurable authentication mechanism priority.

### Impact on test suites and testing automation

* The new tests will extend the existing security coverage in the QE test suite under `quarkus-test-suite/security`. This is preferred because the relevant mechanisms have already been setup.

* **security/oidc-client-mutual-tls** - OIDC vs mTLS authentication priority tests

* **security/jwt** - JWT vs Basic authentication priority tests

* **security/webauthn** - WebAuthn vs Basic authentication priority tests

Note: tests that verify opposite priority directions will use separate test classes and properties files to avoid conflicting application configuration.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should be less than 5 minutes.

* Tests will be executed on baremetal in JVM and native mode. 

* OpenShift coverage will be added for SmallRye JWT vs Basic authentication.

* WebAuthn OpenShift coverage is not planned because the existing scenario is disabled see: https://github.com/quarkus-qe/quarkus-test-suite/issues/1500


## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream issue and pull request.
* Reviewed documentation on authentication mechanism priority.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/security-authentication-mechanisms#authentication-mechanisms-priority