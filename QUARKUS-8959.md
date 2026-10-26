# QUARKUS-8959 - MCP client according to 2026-07-28

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8959

Upstream PRs:
- https://github.com/langchain4j/langchain4j/pull/5881
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2723

Documentation: https://docs.quarkiverse.io/quarkus-langchain4j/dev/mcp.html

Related plans:
- [QUARKUS-8895](QUARKUS-8895.md) Include Quarkus MCP Server 2.x in IBQ 3.40
- [QUARKUS-7049](QUARKUS-7049.md) Full support for MCP Client

The LangChain4j MCP client, used by Quarkus LangChain4j, now supports MCP revision `2026-07-28` in addition to
`2025-11-25`. The new revision is stateless: there is no initialization handshake and no session, and every request
carries its own protocol version. When no protocol version is configured, the client detects which revision the
server supports and prefers `2026-07-28`. This changes the behavior of existing applications that do not set a
protocol version, so we need to verify that they keep working and that the new client features behave correctly.

The feature is available since Quarkus LangChain4j 1.13.0 / Quarkus Platform 3.39.1.
[QUARKUS-8959](https://redhat.atlassian.net/browse/QUARKUS-8959) targets Quarkus 4.3.

## Scope of the testing

In scope:

- Protocol version detection when no version is configured, over stdio and WebSocket, and with mixed protocol
  versions in one application. Applications that do not set a protocol version are the ones affected by the change
  without any action from the user.
- Authentication during protocol version detection. Without a session, the client sends credentials on every request,
  including the detection request.
- Resource subscriptions with the new client API, the client methods that depend on the protocol version.

Out of scope:

- Server behavior on both protocol versions is covered by [QUARKUS-8895](QUARKUS-8895.md), including
  multi-round-trip requests, required request headers and server-side subscriptions.
- Detection against servers that support only `2025-11-25`, or that do not answer the detection request correctly.
- Transport details better suited for upstream integration tests, proposed as upstream contributions:
  - Headers that the client derives from tool parameters (`Mcp-Param-*`).
  - Static and dynamic custom headers and TLS on `2026-07-28`.
  - Tool execution timeout and cancellation.
- Tracing, metrics and health checks for the new operations.

## Getting familiar with the feature

- Reviewed the upstream PRs and the MCP client documentation.
- Compared the client behavior with both protocol versions, and checked the client configuration exposed by Quarkus.
- Reviewed the upstream tests, the TS `ai/mcp` module and the related test plans.

## Existing test coverage

Upstream LangChain4j and Quarkus LangChain4j run most MCP client tests for both protocol versions, and cover version
detection over HTTP, including against a server that supports only `2025-11-25`. The gaps are:

- Version detection is not tested over stdio and WebSocket.
- Resource subscriptions on `2026-07-28` are tested only over HTTP.
- No tests for the client methods that fail on the wrong protocol version, or for the `subscribe-to-*-list-changes`
  configuration set to `false`.

The TS `ai/mcp` module, with [quarkus-test-suite#3219](https://github.com/quarkus-qe/quarkus-test-suite/pull/3219),
sets the existing scenarios to `2025-11-25`, adds `2026-07-28` scenarios on all transports, and verifies version
detection over HTTP. It does not cover detection over stdio and WebSocket, mixed protocol versions, authentication
failures during detection, or the client subscription API.

## Planned coverage

All tests go into the existing `ai/mcp` module, reusing its server and client applications. The client application
gets new endpoints for the subscription operations.

- **Protocol version detection**
  - Verify that a client without a configured protocol version selects `2026-07-28` against the 2.x server on stdio
    and WebSocket (HTTP is already covered).
  - Verify an application with two clients, one set to `2025-11-25` and one detecting the version, both work
    against the same server.
- **Authentication during detection**
  - Verify that missing or invalid credentials report an authentication failure at startup.
  - Verify that valid credentials from a named OIDC client allow the detection and the client selects `2026-07-28`.
- **Subscriptions and list changes**
  - Verify resource subscriptions with the new client API on stdio and WebSocket.
  - Verify that the client methods that no longer exist in `2026-07-28` fail against a `2026-07-28` server, and that
    the new subscription methods fail against `2025-11-25`.
  - Verify the new `subscribe-to-*-list-changes` configuration: when enabled, list changes on the server are visible
    to the client; when disabled, the client keeps the cached lists.

## Impact on test suites and testing automation

No new modules or CI jobs.

Estimated 4 new test classes (stdio, WebSocket, HTTP, and authentication), plus an OpenShift variant of the
authentication class, with about 15 test methods, and 2 or 3 new classes in the applications.

## Impact on resources

The added tests should have **low to moderate impact** on resources:

* An estimated execution time increase of 3 to 4 minutes on baremetal in JVM mode.
* Native mode and OpenShift take longer: an estimated 10 minutes each.
* Tests will be executed on baremetal and OpenShift, in JVM and native mode.
* stdio tests run on bare metal only, because the client starts the server as a local process.

## Contacts

- Tester: Roberto Cortez <radcortez@ibm.com>

## References

- https://docs.quarkiverse.io/quarkus-langchain4j/dev/mcp.html
- https://github.com/langchain4j/langchain4j/pull/5881
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2723
- https://modelcontextprotocol.io/specification/2026-07-28
- https://modelcontextprotocol.io/specification/2025-11-25
- [QUARKUS-8895](QUARKUS-8895.md)
- [QUARKUS-7049](QUARKUS-7049.md)
