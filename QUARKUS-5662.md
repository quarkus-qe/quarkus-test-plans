# QUARKUS-5667: OIDC and OIDC Client: Support JWT bearer client authentication using client assertion loaded from filesystem

Jira: https://issues.redhat.com/browse/QUARKUS-5662

Upstream documentation:
* https://quarkus.io/guides/security-oidc-code-flow-authentication#oidc-provider-client-authentication
* https://quarkus.io/guides/security-openid-connect-client-reference#jwt-bearer

Code changes: 
* https://github.com/quarkusio/quarkus/pull/45131

This feature cover usage of JWT token loaded from file on filesystem.
There are two cases - using token in general oidc settings and in oidc-client.

## Scope of the testing
Tests will require OIDC provider (keycloak in our case) to provide and validate JWT tokens. 

### General
* A new tests will be implemented in `security/keycloak-oidc-client-extended`.
* Tests will cover these token-cases for both oidc and oidc-client:
  * Non-existing file with token.
  * Empty file with token.
  * File containing other data than token.
  * File containing valid token.

### Impact on resources
* New test should be executed within a minute or single minutes for both bare metal JVM and native

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>