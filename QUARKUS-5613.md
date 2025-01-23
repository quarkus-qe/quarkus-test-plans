## Links to the resources
- JIRA ticket: https://issues.redhat.com/browse/QUARKUS-5613
- GH PRs related to the JIRA issue: https://github.com/quarkusio/quarkus/pull/43283 https://github.com/quarkusio/quarkus/issues/42117
- TP/TD PR: https://github.com/quarkus-qe/quarkus-test-plans/pull/193
- Guides modified and related:
  * https://quarkus.io/guides/security-openid-connect-client-reference
  * https://quarkus.io/guides/security-oidc-code-flow-authentication
  * https://quarkus.io/guides/security-openid-connect-client-registration

## Scope of the testing

Our main testing scope is to ensure that the new OIDC response filter mechanism works as intended for various OIDC endpoints, ensuring that is possible intercept and handle responses
from various OIDC endpoints. This complements the already available OidcRequestFilter functionality.
This kind of validation is possible by asserting that:
    - response body can be read and serialized,
    - status code is correct,
    - response headers are correct
    - and request properties matches the ones recorded in the request filter

This scope will be covered on areas related to:
   - Security - ensuring integration with OIDC provider (as Keycloak) plus Quarkus OIDC and OIDC Client extensions.
   - Logging /Tracing -  to enable logs only at finer levels for our specific filter classes to confirm that the filter is triggered. This helps debug the response filter’s internal behavior.

The test coverage will be verified for JVM, native mode, and Openshift.
The testing include OIDC Client (quarkus-oidc), OIDC Client (quarkus-oidc-client) and OIDC client registration (quarkus-oidc-client-registration) extensions.

### What to Test - Scenarios
We are focusing on these three scenarios (Token,User info, Client Registration) as they represent the most common endpoints for Quarkus OIDC users.

#### Token Endpoint Response Filtering
 - Verify that a custom OIDC response filter (TokenEndpointResponseFilter) is triggered immediately after a successful token refresh.
 - In particular this scenario will use `quarkus-oidc` and `quarkus-oidc-client`
 - Validation
    * Ensure that filter logs messages contains "token have been refreshed".
    * Verify 'OidcResponseContext' exposes the correct property grant_type=refresh_token
    * Assert that the response includes "refresh_token"

####  User Info Endpoint Response Filtering
- Demonstrate that an OIDC response filter can intercept the User Info endpoint response, for instance verifying UserInfo claims.
-  Validation
    * Confirm logs indicating the filter triggered on `/userinfo` (Keycloak path: `/realms/{realm}/protocol/openid-connect/userinfo`).
    * Filter runs and logs claim details(`sub`, `preferred_username`)
    * JSON body response includes the details.
    * Logs show filter intercepting `/userinfo` response
Note:  We do not test Keycloak’s user info correctness; only that the filter sees the response code/body.

#### Client Registration Endpoint Response Filtering
- Create a filter(OidcEndpoint.Type.CLIENT_REGISTRATION) intercepts responses to client registration calls.
- Validation
  * Registration data, for instance check filter logs newly registered `client_id` or `client_name`
  * Check if filter can detect a missing invalid property in the returned JSON
  * Error handling, if Keycloak return a 401 or 4xx the filter logs or throws an exception appropriately


## Getting familiar with the feature

- Check and looking through the guides:

 * https://quarkus.io/guides/security-openid-connect-client-reference

 * https://quarkus.io/guides/security-oidc-code-flow-authentication

 * https://quarkus.io/guides/security-openid-connect-client-registration


## Community status of the extension/feature:
All the extensions used in the module are stable and supported.
 - quarkus-oidc (supported)
 - quarkus-oidc-client(supported)
 - quarkus-oidc-client-registration (It is on experimental status on upstream)

## Automated test development
Extend this test coverage existing security/keycloak-oidc-client-reactive-extended module to:
- Implement test classes covering the token endpoint (`grant_type=refresh_token`), userinfo endpoint, and optionally client registration.
- Verify that each response filter is triggered for the correct endpoint type (token, userinfo, client registration), and only for that type.
- Check logs or exceptions to confirm the corect status code, Json response, etc.

#### Impacted resources
Additional test classes → slight increase in runtime.

## Contacts

* Tester: Jose Carranza <jcarranz@redhat.com>