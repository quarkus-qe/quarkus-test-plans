# QUARKUS-2491 OIDC - Support for Proof Of Key for Code Exchange (PKCE)

JIRA link: https://issues.redhat.com/browse/QUARKUS-2491
Quarkus documentation: https://quarkus.io/guides/security-openid-connect-web-authentication#proof-of-key-for-code-exchange-pkce
PR: https://github.com/quarkusio/quarkus/pull/23423
Upstream issue: https://github.com/quarkusio/quarkus/issues/12856

Support for Proof Of Key for Code Exchange (PKCE), that minimizes the risk of the authorization code interception, has been added in Quarkus 2.8.
Among others, it provides an extra level of protection to Quarkus OIDC web-app applications.
PKCE for an OIDC web-app can be enabled with a `quarkus.oidc.authentication.pkce-required` property and a 32 characters long secret set in `application.properties`.

The secret key will be used for encrypting a randomly generated PKCE `code_verifier` while the user is being redirected with the `code_challenge` query parameter to OpenID Connect Provider to authenticate.
The `code_verifier` will be decrypted when the user is redirected back to Quarkus and sent to the token endpoint alongside the code, client secret and other parameters to complete the code exchange.

## Scope of testing
Keycloak provides integrated support for PKCE. We can set `pkce.code.challenge.method` attribute to `S256` and the provider will fail the code exchange if a SHA256 digest of the `code_verifier` does not match the `code_challenge` provided during the authentication request.

### Existing tests
No QE existing coverage, however `LogoutSinglePageAppFlowIT` already uses PKCE for `account-console` and `security-admin-console` Keycloak clients.
Upstream test coverage seems sufficient, however we need to run tests with Red Hat Single Sign-On and with JVM and native build options supported for RHBQ.

## Automated test development
We should enable PKCE for the `io.quarkus.ts.security.keycloak.oidcclient.reactive.extended.LogoutSinglePageAppFlowIT` test in [Quarkus QE test suite](https://github.com/quarkus-qe/quarkus-test-suite) and its OpenShift alternative.
This way, we have SPA logout flow covered with PKCE and without PKCE (through `io.quarkus.ts.security.keycloak.oidcclient.extended.restclient.LogoutSinglePageAppFlowIT`).
We don't need to create dedicated tests for both classic and reactive extensions as PKCE is handled by `quarkus-oidc` extension before both JAX-RS reactive and classic layer begin processing.

## Impact on test suites and test environment
No impact as PKCE will be enabled in existing test coverage run both in JVM and native mode, on bare metal and in OpenShift (please see `io.quarkus.ts.security.keycloak.oidcclient.reactive.extended.OpenShiftOidcSinglePageAppLogoutFlowIT`).

### Impact on resources:
No change in needed resources for testing.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Future considerations

- We might want to add test coverage for OAuth 2.0 providers that uses PKCE, as Twitter, please see [QUARKUS-2488](https://issues.redhat.com/browse/QUARKUS-2488)

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [OAuth 2.0 Authorization Code Flow with PKCE](https://developer.twitter.com/en/docs/authentication/oauth-2-0/authorization-code)
- [Proof Key for Code Exchange by OAuth Public Clients](https://datatracker.ietf.org/doc/html/rfc7636)