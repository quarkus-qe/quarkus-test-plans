# QUARKUS-7048 Full support for MCP Server
Jira link: https://issues.redhat.com/browse/QUARKUS-7048

## Scope of testing
Verify, that all supported features in Quarkus implementation of MCP server work

### Test cases
The list of supported features (from [1]):
### Server
- Support for core JSON-RPC message types, connection initialization, capability negotiation, and session control.
- Structured data or content (e.g., files, database schemas) exposed by the server to provide context to the model.
- Exposing parameterized URI templates (e.g., file:///{path}).
- Allow clients to subscribe to specific resource URIs and receive notifications when they update.
- Tools and notifications for them
- Prompts and notification for them
- Autocompletion
- Server-client logging and progress
- Pagination
- Cancellations and pings
- Icons
- Sampling
- Roots and notifications
- Elicitation via JSON schema

### Protocols
- StdIO
- Streamable HTTP (SSE)
- Websockets (custom protocol, not described in specification)

## Getting familiar with the feature
- https://modelcontextprotocol.io/specification/2025-11-25
- https://docs.quarkiverse.io/quarkus-mcp-server/dev/index.html
- https://quarkus.io/blog/mcp-server/
- https://docs.langchain4j.dev/tutorials/mcp/

## Impact on test suite

A new module will be added to `ai` folder in Quarkus QE TS.
There will be classes for local tests (for Stdio, HTTP and Websocket connections), and Openshift version for HTTP and Websockets.

### Note:
Quarkus MCP client doesn't support the following:
- Resource Subscriptions
- Autocompletions
- Prompt notifications
- Sampling
- Elicitations
- Progress

Additionally, Icons are not in MCP client spec at all. These features should be verified by other means (eg with direct rest requests). We must also verify, that when client encounters unknown content it fails with clear error. 

## Impact on resources
Running times of tests are expected to be the same as for http-minimum (from ~2 min for jvm run to ~12 min for Native run on OCP)


## Contacts
* Tester: Fedor Dudinsky <fdudinsk@ibm.com>

## Footnotes
[1] https://docs.google.com/document/d/1uI59Bjmt5kjNxLWw3dcXfXxM04yxRK8NArcP-rUrDFQ/edit?tab=t.0#heading=h.hcemzs8ccjae