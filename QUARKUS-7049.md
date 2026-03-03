# QUARKUS-7049 Full support for MCP Client
Jira link: https://issues.redhat.com/browse/QUARKUS-7049

## Scope of testing
Verify, that all supported features in Quarkus implementation of MCP client work

### Test cases
The list of supported features (from [1]):

- Support for core JSON-RPC message types, connection initialization, capability negotiation, and session control.
- Structured data or content (e.g., files, database schemas) exposed by the server to provide context to the model.
- Exposing parameterized URI templates (e.g., file:///{path}).
- Tools and notifications for them
- Prompts
- Pagination
- Roots and notifications for them

### Protocols
- StdIO
- Streamable HTTP (SSE)
- Websockets (custom protocol, not described in specification)

## Getting familiar with the feature
- https://modelcontextprotocol.io/specification/2025-11-25
- https://docs.quarkiverse.io/quarkus-langchain4j/dev/mcp.html
- https://docs.langchain4j.dev/tutorials/mcp/

## Impact on test suite
Will be implemented together with MCP server coverage (see QUARKUS-7048.md)
Extension `io.quarkiverse.langchain4j:quarkus-langchain4j-mcp` should be moved to `supported` in Marete configs

## Impact on resources
No additional impact comparing to server coverage


## Contacts
* Tester: Fedor Dudinsky <fdudinsk@ibm.com>

## Footnotes
[1] https://docs.google.com/document/d/1uI59Bjmt5kjNxLWw3dcXfXxM04yxRK8NArcP-rUrDFQ/edit?tab=t.0#heading=h.hcemzs8ccjae