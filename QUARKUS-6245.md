# QUARKUS-6245 Allow setting Clear-Site-Data on OIDC logout

JIRA: https://issues.redhat.com/browse/QUARKUS-6245

Upstream PR: https://github.com/quarkusio/quarkus/pull/46864

Upstream documentation:
* https://quarkus.io/guides/security-oidc-code-flow-authentication#logout-and-expiration

This feature allows to set [Clear-Site-Data](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Clear-Site-Data) HTTP header on logout from OIDC.

## Scope of testing
Verify that setting the Quarkus config property will result in "Clear-Site-Data" HTTP header being sent to client during logout from OIDC.
The header can have several values, test that multiple values are sent.
Verify that header is sent in RP-initiated a front-channel logout.

## Existing test coverage
There is already a test coverage for code-flow-authentication in `keycloak-oidc-client-extended` module - `AbstractLogoutSinglePageAppFlowIT`.

### Impact on test suite
New test case will be added into `keycloak-oidc-client-extended` module for both bare metal and OCP.

### Impact on resources
Test will run on bare metal and OCP, JVM and native.
It will run as part of already executed test class so impact on resources will be negligible.

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>