# QUARKUS-6550 - Support for customizing request and response body in OIDC filters

JIRA: https://issues.redhat.com/browse/QUARKUS-6550

Upstream issues: https://github.com/quarkusio/quarkus/issues/40258 https://github.com/quarkusio/quarkus/issues/49041

Upstream PR: https://github.com/quarkusio/quarkus/pull/49042

## Scope of the testing
The PR #49042 introduces the ability to customize request and response bodies in OIDC filters, addressing limitations
when integrating with non-standard OAuth2/OIDC providers.

- **Request body customization in OIDC filters** can now modify request bodies sent to OIDC providers using the new `OidcRequestContext.requestBody(Buffer)` method.
This enables adding custom parameters or transforming requests for providers with non-standard requirements.
- **Response body customization in OIDC filters** can transform response bodies from OIDC providers using `OidcResponseContext.responseBody(Buffer)`. 
This is crucial for handling providers that return tokens in nested JSON structures or use non-standard formats.
- **Filter targeting**
The existing `@OidcEndpoint` annotation mechanism works with the new body customization feature to target specific OIDC endpoints (TOKEN, USERINFO, etc).

### What will be tested
 + **Non-standard OAuth2 provider integration** - Test scenarios simulating real-world providers that require request/response transformation.
 + **Filter chain behavior** - Verify that multiple filters can sequentially modify bodies without conflicts.
 + **Content type handling** - Ensure proper handling of different body formats (JSON, form-encoded).
 + **Error scenarios** - Validate graceful handling of malformed bodies and transformation failures.

## Existing test coverage

* **Upstream Test coverage:** was implemented in the PR (# - https://github.com/quarkusio/quarkus/pull/49042/files) includes 
unit tests demonstrating the filter capabilities.
* **QE Test Suite:** currently covers OIDC client and filter basics, but there are no tests yet covered in our test suite for
body customization scenarios.

### Impact on test suite

New test classes will be added to the `security/keycloak-oidc-client-extended` module,
and will be executed on both JVM and native modes on both bare-metal and OCP.

### Impact on resources

The expected additional execution time will be increased about 2 minutes for JVM mode and 5 minutes for native mode.
On OCP, the execution time is expected to increase by around 5 minutes for JVM mode and 11 minutes for native mode.

## Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Review of the PR implementation and test coverage
- Review the quarkus guides regarding OIDC filters

## References

- https://quarkus.io/guides/security-oidc-code-flow-authentication#code-flow-oidc-request-filters
- https://quarkus.io/guides/security-oidc-expanded-configuration#request-response-and-redirect-filters
- https://quarkus.io/guides/security-openid-connect-client-reference#oidc-client-ref-oidc-request-filters
- https://quarkus.io/guides/security-openid-connect-client-registration#oidc-client-registration-oidc-request-filters
- https://quarkus.io/guides/security-openid-connect-client-registration#oidc-client-registration-oidc-response-filters

## Contacts

* Tester: Jose Carranza <jcarranz@redhat.com>