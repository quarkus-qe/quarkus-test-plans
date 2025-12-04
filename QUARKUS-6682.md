## Links to the resources
- JIRA ticket: https://issues.redhat.com/browse/QUARKUS-6682
- GitHub pull request related and the JIRA issue:  
  https://github.com/quarkusio/quarkus/issues/46697

  https://github.com/quarkusio/quarkus/pull/49122

## Scope of the testing

The [PR #49122](https://github.com/quarkusio/quarkus/pull/49122) introduces the ability to restrict OIDC request and response filters to specific authentication flows (Bearer Token vs Authorization Code) and specific tenants.

This enhancement addresses the need to apply filters selectively based on the authentication mechanism or the tenant context being used.

### What will be tested

- **Bearer token flow isolation** - Verify that filters annotated with `@BearerTokenAuthentication` are executed only during bearer token authentication (service tenant)
    and not during web-app logins.

- **Authorization Code flow isolation** - Verify that filters annotated with `@AuthorizationCodeFlow` are executed only during web-app login flows (webapp tenant) 
    and not during API bearer token calls.

- **Tenant Feature isolation** - Verify that filters annotated with `@TenantFeature("service-tenant")` are only executed when requests are routed to the specified tenant and ignored for
    other tenants.

- **Default behavior (Regression)** - Confirm that filters without any flow or tenant annotations continue to trigger for all flows and all tenants, maintaining backward compatibility.

- **Combined restrictions** - Test filters that use both flow and tenant annotations simultaneously (e.g., `@BearerTokenAuthentication` + `@TenantFeature("service-tenant")`).

- **Negative scenarios** Validate behavior when:
    - A filter restricted to Bearer flow is not invoked (filter logic is never executed) when processing Authorization Code flow requests.
    - A filter restricted to one tenant is not invoked when processing requests for a different tenant.
    - Filter state isolation between different authentication flows

The test coverage will be verified for JVM, native mode, and Openshift.

## Existing test coverage

- **Upstream Test coverage:** The PR adds comprehensive unit/integration tests in `OidcRequestAndResponseFilterTest.java` within the `extensions/oidc/deployment` module, 
covering permutations of `@BearerTokenAuthentication`, `@AuthorizationCodeFlow`, and `@TenantFeature` filters.

- **QE Test Suite:** Currently, coverage exists for `@OidcEndpoint` targeting in OIDC filters, but there are no tests yet covering the new flow-specific and tenant-specific filter restrictions.

### Impact on test suite

New test classes will be added to the `security/keycloak-multitenant` module, and will be executed on both JVM and native modes on both bare-metal and OCP.

The tests will leverage the existing multi-tenant configuration in the module:
- `service-tenant` (application-type=service) for Bearer token flow tests
- `webapp-tenant` (application-type=web-app) for Authorization Code flow tests
- `jwt-tenant` (application-type=web-app) for additional Authorization Code flow scenarios

### Impact on resources

The expected additional execution time will be increased approximately:
- 2 minutes for JVM mode and 4 minutes for native mode on bare-metal
- 3 minutes for JVM mode and 6 minutes for native mode on OCP

## Getting familiar with the feature

- Check and looking through the specific PR https://github.com/quarkusio/quarkus/pull/49122/ and guides:

* https://quarkus.io/guides/security-oidc-code-flow-authentication
* https://quarkus.io/guides/security-openid-connect-multitenancy
* https://quarkus.io/guides/security-oidc-bearer-token-authentication


## Community status of the extension/feature:
The extensions used in the module
- quarkus-oidc (supported)

## Contacts

* Tester: Jose Carranza <jcarranz1@ibm.com>