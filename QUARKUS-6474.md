# QUARKUS-6474 - OIDC Client: Add periodic asynchronous tokens refresh for performance critical applications

JIRA: https://issues.redhat.com/browse/QUARKUS-6474

Upstream issue: https://github.com/quarkusio/quarkus/issues/46673

Upstream PR: https://github.com/quarkusio/quarkus/pull/48296

### Scope of changes in PR #48296
The PR #48296 introduces support for asynchronous periodic refresh of OIDC client tokens.
By configuring `quarkus.oidc-client.refresh-interval`, applications can proactively refresh tokens in the background at fixed intervals, instead of refreshing only when tokens expire.
This ensures that requests always have a valid token without waiting for a refresh to complete.

## Scope of testing
### Periodic asynchronous refresh

Verify that tokens are refreshed automatically in the background according to the configured interval.
- Configure `quarkus.oidc-client.refresh-interval`.
- Start the application and acquire the initial token.
- Wait until the configured interval passes.
- Call a protected endpoint and retrieve the token value.
- Verify the token has changed after the interval, confirming the background refresh.

### Direct Token Retrieval API unaffected

Ensure that calls to `OidcClient#getTokens()` are not influenced by the background refresh interval.
- Configure `quarkus.oidc-client.refresh-interval`.
- Call `OidcClient#getTokens()` directly in application code and store the token.
- Wait until the refresh interval passes.
- Call `OidcClient#getTokens()` again and check that the token value remains unchanged.

## Existing test coverage

- **Upstream Test coverage:** implemented in [PR #48296](https://github.com/quarkusio/quarkus/pull/48296), includes a single integration test verifying background token refresh and that subsequent requests use the refreshed token.
- **QE Test Suite:** currently covers OIDC client and filter basics, but there are no tests yet for periodic asynchronous token refresh or direct API usage.


### Impact on test suite

New test classes will be added to the `security/keycloak-oidc-client-extended` module,
and will be executed on both JVM and native modes on both bare-metal and OCP.

### Impact on resources

The expected additional execution time will be increased about 30 seconds for JVM mode and about 2 minutes for native mode.
On OCP, the execution time is expected to increase by around 1,5 minutes for JVM mode and 3 minutes for native mode.

## Getting familiar with the feature

- Review [PR #48296](https://github.com/quarkusio/quarkus/pull/48296) and its test coverage.
- Review [OIDC client documentation](https://quarkus.io/guides/security-openid-connect-client-reference#refresh-access-tokens).
- Review user request in [discussion #46644](https://github.com/quarkusio/quarkus/discussions/46644).

## References

- https://quarkus.io/guides/security-openid-connect-client-reference#refresh-access-tokens
- https://github.com/quarkusio/quarkus/issues/46673
- https://github.com/quarkusio/quarkus/pull/48296
- https://github.com/quarkusio/quarkus/discussions/46644

## Contacts
* Tester: Georgii Troitskii <gtroitsk@ibm.com>