# QUARKUS-7932 - Support named OIDC clients in OidcClientMcpAuthProvider

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7932
``
Upstream PR: https://github.com/quarkiverse/quarkus-langchain4j/pull/2442

Adds quarkus.langchain4j.mcp.<client>.oidc-client-name to bind each MCP client to a distinct named OIDC client configuration. Previously, OidcClientMcpAuthProvider only used the default OIDC client, making it impossible to connect to multiple MCP servers with unrelated OIDC issuers in the same application.

## Scope of the testing

We should add the following checks for oidc+mcp:
- A client can connect to MCP server, which uses some SSO provider
- A client can connect to different servers, with each using a different SSO provider
- In any configuration above, a client can not connect, if it has invalid credentials


## Existing test coverage

Upstream tests cover the scenarios above but only on unit test level.
No coverage for OIDC+MCP in Quarkus TS.

### Impact on test suites and testing automation

Two new basic test classes will be added to ai/mcp module. One checking, that client can connect to a server using OIDC, another that a separate OIDC provider can be specified per MCP server. Each one should have an OpenShift version. 

### Impact on resources

Running time should grow for ~10 minutes in the worst case (native+Openshift), since we will need to start two keycloak instances and three applications (a client and two servers)

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documentation

## Contacts

* Tester: Fedor Dudinskii fdudinsk@ibm.com

## References
* https://docs.quarkiverse.io/quarkus-langchain4j/dev/mcp.html#_authorization
* https://docs.quarkiverse.io/quarkus-mcp-server/dev/guides-client-integration.html