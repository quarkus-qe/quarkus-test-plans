# QUARKUS-7174 Add support for OAuth 2.0 Pushed Authorization Requests to OIDC extension

JIRA: https://issues.redhat.com/browse/QUARKUS-7174

Add support for OAuth 2.0 Pushed Authorization Requests to OIDC extension

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/51799

Documentation:
- https://quarkus.io/guides/security-oidc-code-flow-authentication#pushed-authorization-requests

## Scope of testing
The QUARKUS-7174 adding the ability to use Pushed Authorization Requests (PAR).
The PAR is enhancement for OAuth 2.0 and OpenID Connect.
The normal workflow of authorization works as they redirect user to login form and values for parameter as `client_id`, `state` and other are exposed in url.
This can be altered, captured etc.
The PAR works similar but on the initial request application push the request directly to authorization server and the server return redirect address without the exposed value, there will be only identifier in url.

Testing of PAR will be done using Keycloak as authorization server.
The tests will include check if the Quarkus correctly redirect url containing only the identifier and not the values.
Further tests will verify whether the user has access to secure endpoints using `client_secret` and JWT.

This feature also contains the case where `quarkus.oidc.authentication.par.enabled` is not set and PAR is enabled by default, when the authorization server contains `require_pushed_authorization_requests` in discovery metadata.
This case won't be tested, as it's not possible to set `require_pushed_authorization_requests` to `true` at `/realms/<realn-name>/.well-known/openid-configuration` using Keycloak.


## Automated test development

Upstream coverage:
* There are 3 tests, covering basic functionality of this feature.

The tests will cover these scenarios:
* Check if Quarkus and Keycloak communicate using back channel and Quarkus redirect user to url with identifier only
* Check when `quarkus.oidc.authentication.par.enabled` is set, the user is authorized access secured endpoint.
* Check when `quarkus.oidc.authentication.par.enabled` is set, the unauthorized user cannot access the secure endpoint.
* Set the `quarkus.oidc.authentication.par.path` to non-existent endpoint to verify, that this property is respected.
  * The Keycloak don't have option to change `pushed_authorization_request_endpoint` so this property can be tested only for negative results.


### Impact on TS and resources
The tests will be added to [`keycloak-webapp`](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security/keycloak-webapp) and [`keycloak-jwt`](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security/keycloak-jwt) modules.

The tests will be executed on bare-metal and OpenShift in JVM and Native mode.
The expected time increase will be standard as there won't be addition of module and the PAR properties are not build time properties.

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>