# QUARKUS-7341 - OIDC: Add basic support for rich authorization requests

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7341

Upstream issue: https://github.com/quarkusio/quarkus/issues/52286

Upstream PR: https://github.com/quarkusio/quarkus/pull/52353

Rich Authorization Requests (RFC 9396) allow sending structured `authorization_details` in the OIDC authorization request instead of simple scopes.
Quarkus OIDC now supports RAR through configuration properties, with string and array field types supported.

## Scope of the testing

Verify that `authorization_details` is correctly sent during the OIDC authorization code flow and that the token response contains the granted details.
Testing will use Keycloak with the OID4VC feature enabled.

Tests will cover:

* RAR configured via `quarkus.oidc.authentication.rar.type`, `.simple.<field>`, and `.array.<field>` appends `authorization_details` to the authorization redirect.
* The token response contains `authorization_details` with the expected type, string fields, and array fields.
* An `OidcResponseFilter` with `@TenantFeature` and `@OidcEndpoint(Type.TOKEN)` captures `authorization_details` from the token response.
* An `OidcRedirectFilter` constructs `authorization_details` containing multiple authorization detail objects, each with its own type and fields, which configuration properties cannot express since they only support a single object.
* Wrong or missing `type`, incorrect `locations`, and mismatched field types produce clear error messages in the log.

## Existing test coverage

Upstream tests: https://github.com/quarkusio/quarkus/tree/main/integration-tests/oidc-client-registration/src/test/java/io/quarkus/it/keycloak

Upstream coverage verifies RAR via configuration with Keycloak dev services and OID4VC enabled.
It does not test with a standalone Keycloak or on OpenShift.

The QE test suite does not contain any tests for Rich Authorization Requests.

### Impact on test suites and testing automation

New tests will be added to `security/keycloak-oidc-client-extended`.
The module already has Keycloak infrastructure, code flow tests, and response filter patterns.
Keycloak will need the OID4VC feature enabled and a dedicated RAR tenant configured.

### Impact on resources

Tests will be executed on baremetal and OpenShift in JVM and native mode.
The new tests reuse the existing Keycloak container, so the impact should be minimal.
The estimated execution time increases by a few minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Reviewed upstream issue, pull request and test coverage
- Reviewed RFC 9396 (OAuth 2.0 Rich Authorization Requests)
- Reviewed Quarkus OIDC code flow authentication guide
- Will review documentation and suggest improvements if found during testing
- Will check for overlap or integration points with other Quarkus extensions and discuss with the feature developer
- Focus on exploratory testing of the feature

## Contacts

* Tester: Slimane Abzar <sabzar@redhat.com>

## References

- https://quarkus.io/guides/security-oidc-code-flow-authentication
- https://datatracker.ietf.org/doc/html/rfc9396