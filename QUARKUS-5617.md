# QUARKUS-5617 Support for two or more authentications for a single request

JIRA: https://issues.redhat.com/browse/QUARKUS-5617

Support for two or more authentications for a single request

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/42935

Upstream issue:
- https://github.com/quarkusio/quarkus/issues/31328

Documentation:
- https://quarkus.io/guides/security-authentication-mechanisms#combining-authentication-mechanisms

## Scope of testing
Verify that Quarkus with inclusive authentication enabled requires authentication of all configured mechanisms simultaneously.
The tested combination will be Basic mechanism and OIDC mechanism.


## Upstream test coverage
The upstream contains tests for a combination of mTLS and OIDC mechanisms.


## Automated test development
The tests will verify that when `quarkus.http.auth.inclusive` is enabled, requests must be authenticated by all configured mechanisms.
The testing combination will be Basic and OIDC authentication.

The test cases will include:
- A request with both authentication mechanisms to an endpoint where the user has permissions.
- A request with both authentication mechanisms to an endpoint where the user lacks permissions.
- Requests using only one authentication mechanism.


### Impact on TS and resources
These tests will be added to the `security/keycloak` module and executed in both JVM and native modes on bare metal and OCP.
The expected increase in execution time for bare metal is approximately 45 seconds for JVM mode and 5 minutes for native mode.
On OCP, the execution time is expected to increase by around 2 minutes for JVM mode and 7 minutes for native mode.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
