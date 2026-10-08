# QUARKUS-8895 - Include Quarkus MCP Server 2.x in IBQ 3.40

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8895

Quarkus MCP Server 2.x adds support for MCP protocol revision `2026-07-28`
while retaining older revisions. The newer revision removes protocol sessions
and the initialization handshake; requests carry their protocol version and client
capabilities instead. We need to verify compatibility with existing applications
and cover the changed protocol behavior in IBQ 3.40.

## Scope of the testing

Testing will cover Streamable HTTP, stdio and WebSocket, with explicitly selected
MCP revisions. WebSocket is an additional Quarkus transport, outside the standard
MCP transport specification. The work will follow these priorities:

### 1. Verify existing coverage with MCP 2025-11-25

- Retain existing test coverage for `2025-11-25` across all currently tested
  transports and client paths.
- Explicitly pin the protocol revision for every existing client path, including
  LangChain4j clients, the direct WebSocket client and the Java SDK, and verify
  that the selected revision is used.
- Investigate the disabled roots checks and restore older-revision coverage where
  supported, using a valid roots URI.

### 2. Add coverage for MCP 2026-07-28

Add `2026-07-28` coverage across Streamable HTTP, stdio and WebSocket, matching
the existing functional scope.

- Basic operations currently covered on all three transports: tools, resources
  and resource templates, prompts, dynamic tool changes and client log delivery.
- Advanced features currently covered through WebSocket: icons, resource
  subscriptions, prompt notifications, completion, progress, dedicated logging
  checks, sampling and elicitation using scripted client responses.
- Tool pagination, currently checked through WebSocket and the Java SDK on HTTP.
- Authentication and named OIDC provider routing, currently covered on HTTP.

Adapt the existing scenarios to the stateless protocol and verify its observable
behavior.

- Verify server discovery returns supported revisions and capabilities, and
  operations work without the older initialization handshake using per-request
  metadata. Check rejection of missing required metadata and removed methods.
- On Streamable HTTP, check required request headers and header/body consistency,
  requests without session IDs, and rejection of the former GET/DELETE lifecycle.
- Check result type and server identity metadata, plus default and configured
  cache hints on applicable results.
- Verify the replacement subscription flow delivers opted-in change notifications
  and filters unrequested notifications.
- Multi-round-trip requests (MRTR): the server requests client input, and the
  client retries the operation with its response and request state.
- Verify logging uses the request's log level rather than a session-wide setting.

Use McpAssured for direct-server coverage. Retain LangChain4j integration and
independent Java SDK interoperability tests; verify client support before extending
their newer-revision coverage and update the SDK to the latest available release.
After establishing the pinned `2025-11-25` baseline, replace the custom `Session`
client used by advanced `WebSocketIT` tests with McpAssured. Use McpAssured for
direct-server coverage of both protocol revisions, retaining the existing
assertions. Keep LangChain4j and official Java SDK tests as independent-client
integration coverage.

Strengthen assertions and add representative negative cases for the operations
and protocol changes covered above.

## Existing test coverage

The `ai/mcp` module covers basic operations through LangChain4j on all three
transports, advanced server features through a direct WebSocket client, and
selected HTTP operations through the official Java SDK. It also covers OIDC token
propagation, token acquisition and named provider routing.

The Quarkus MCP Server project has unit, Quarkus application and integration tests for server features
and transports. These include stateless discovery and requests on HTTP, stdio
and WebSocket, subscriptions, caching, schema validation and authorization.
This work will retain the existing LangChain4j and independent Java SDK
integration coverage while adding explicit coverage for the two protocol revisions.

## Impact on test suites and testing automation

No new modules or CI jobs will be added. All planned test development will go
into the existing `ai/mcp` module.

Existing scenarios will be updated to pin and verify `2025-11-25`. Coverage for
`2026-07-28` will add test methods and, where separate application configuration
is needed, new scenario classes.

Existing HTTP/WebSocket and OIDC scenarios will be retained for JVM, native and
OpenShift testing. Newer-protocol scenarios will follow the same applicable execution
modes.

## Impact on resources

The added tests are expected to have low resource impact. Based on local JVM
execution, the additional coverage is estimated to increase execution time by
approximately 15 minutes, allowing for slower CI machines. This estimate
**excludes** native builds and OpenShift execution. No new CI jobs or external services
are required.

Development effort is estimated at approximately 5 MD, covering
implementation, local verification and review adjustments. Investigation of
upstream issues may require additional effort.

## Getting familiar with the feature

- Reviewed existing MCP and LangChain4j integration tests.
- Compared protocol revisions and inspected the target extension sources.
- Reviewed versioned documentation and checked advertised features against the
  snapshot implementation.

## Contacts

- Tester: Matej Novotny <manovotn@ibm.com>

## References

- [Quarkus MCP Server 2.0.x documentation](https://docs.quarkiverse.io/quarkus-mcp-server/2.0.x/index.html)
- [Testing with McpAssured — MCP Server 2.0.x](https://docs.quarkiverse.io/quarkus-mcp-server/2.0.x/guides-testing.html)
- [Quarkus MCP Server source — 2.0.x branch](https://github.com/quarkiverse/quarkus-mcp-server/tree/2.0.x)
- [MCP 2025-11-25 specification](https://modelcontextprotocol.io/specification/2025-11-25)
- [MCP 2026-07-28 — Key Changes](https://modelcontextprotocol.io/specification/2026-07-28/changelog)
- [Quarkus LangChain4j MCP integration — dev documentation](https://docs.quarkiverse.io/quarkus-langchain4j/dev/mcp.html)
- [QUARKUS-7048 — Full support for MCP Server](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-7048.md)
- [QUARKUS-7049 — Full support for MCP Client](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-7049.md)
- [QUARKUS-7932 — Support named OIDC clients in OidcClientMcpAuthProvider](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-7932.md)
