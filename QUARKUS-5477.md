# QUARKUS-5477 Add OIDC Client SPI

JIRA: https://issues.redhat.com/browse/QUARKUS-5477

Add OIDC Client SPI

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/42879

Upsteram issue:
- https://github.com/quarkusio/quarkus/issues/42878

Documentation:
- https://quarkus.io/guides/security-openid-connect-client-reference#oidc-client-spi

## Scope of testing
The `quarkus-oidc-client-spi` needs either the `quarkus-oidc-client` or `quarkus-oidc-client-deployment` dependency to be present.
The implemented tests will validate the `TokenProvider` ability to generate access tokens for the default OIDC client.
This implementation excludes access token provision for custom OIDC clients.


## Automated test development
- Test suite
  - The `TokenProvider` tests will check:
    - Verify successful access to a protected endpoint using a generated token.
    - Confirm the protected endpoint returns a 401 Unauthorized HTTP status code when token expire.
    - Verify that the token is renewed and that the new token can continue to access the protected endpoint.
    - Assert that token fails to authorize access to a protected endpoint restricted to a different user role.
  - The tests for `TokenProvider` will be added to `security/keycloak-oidc-client-reactive-basic` module and `security/keycloak-oidc-client-basic` module.
  - For both mentioned modules the Keycloak token will have token expiration set in seconds.

### Impact on TS and resources
The new tests will have minimal impact on execution time (roughly 30 seconds per module) for all platforms and modes.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
