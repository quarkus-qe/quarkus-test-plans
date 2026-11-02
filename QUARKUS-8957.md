# QUARKUS-8957 - Initial support for java-mcp-annotations

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8957

Upstream issue: https://github.com/quarkiverse/quarkus-mcp-server/issues/823

Upstream PRs:
- https://github.com/quarkiverse/quarkus-mcp-server/pull/869
- https://github.com/quarkiverse/quarkus-mcp-server/pull/1144

Documentation: 
- https://docs.quarkiverse.io/quarkus-mcp-server/dev/guides-implementing-tools.html
- https://docs.quarkiverse.io/quarkus-mcp-server/dev/release-notes.html)

Community status: Supported

Related plans:
- [QUARKUS-8895](QUARKUS-8895.md) Include Quarkus MCP Server 2.x in IBQ 3.40
- [QUARKUS-7048](QUARKUS-7048.md) Full support for MCP Server
- [QUARKUS-8959](https://github.com/quarkus-qe/quarkus-test-plans/pull/336) MCP client according to 2026-07-28

Quarkus MCP Server now supports the vendor-neutral `org.mcpjava` annotations from java-mcp-annotations, next to its
own `io.quarkiverse.mcp.server` annotations. Applications can declare tools, prompts, resources, resource templates
and completions with either set, or mix both. The upstream issue first proposed replacing the Quarkus annotations and
then settled on supporting both APIs "during the transitional period". The maintainer clarified in the issue that the
Quarkus annotations are not going to be dropped at this time, because the standard API covers only a subset of the
functionality. There is no follow-up issue to remove them and no timeline. We need to verify that the standard
annotations reach the same server behavior as the Quarkus ones, and that they keep working in RHBQ/IBQ releases.

The feature is available since Quarkus MCP Server `2.0.0.Beta2` and is present in every 2.0.x release, including the
`2.0.3` referenced by the platform.

## Scope of the testing

In scope:

- Verification that the standard annotations work in RHBQ/IBQ builds, and regression coverage against z-stream
  updates once they do.
- Streamable HTTP only, because the annotations are processed the same way for all transports.
- An application that mixes both annotation sets.

The standard annotations are the preferred set for new coverage, but the existing Quarkus-annotated tests stay as
they are. Both APIs are supported, upstream does not plan to drop the Quarkus annotations because the standard API
covers only a subset of the functionality, and the Quarkus annotations are what the product ships and what users have
in existing applications, so dropping their coverage would leave the shipped API untested.

Out of scope:

- The MCP protocol and the Quarkus annotations, covered by [QUARKUS-8895](QUARKUS-8895.md) and
  [QUARKUS-7048](QUARKUS-7048.md).
- MCP features the standard API does not define, except when used from a standard-annotated method.
- Areas where upstream already tests the Quarkus annotations, proposed as upstream contributions:
  - stdio and WebSocket, by adding standard-annotated features to the existing integration tests of each transport,
    which also run in native mode.
  - Multiple named server configurations, with standard-annotated features bound to each one.
  - Method-level security on standard-annotated tools.
  - Error handling with the standard exception.
  - Metrics and tracing for standard-annotated calls.
  - Dev mode reload of standard-annotated features.

Extension interactions (security, metrics, tracing and multiple named servers) apply to feature methods regardless of
the annotation set, so they are covered by the upstream contributions above.

## Getting familiar with the feature

- Reviewed the upstream PR and its tests, and the annotations and types of the standard API.
- Reviewed the upstream test modules per transport, the native, tracing and authorization integration tests, and the
  build-time validation of feature methods.
- Reviewed the existing `ai/mcp` module coverage and the related test plans.
- Version under test: `org.mcpjava:mcp-server-api` 1.0.0, the only non-beta release of the standard API.

## Existing test coverage

Upstream Quarkus MCP Server tests the happy path for tools, prompts, resources, resource templates and completions. All
tests run over Streamable HTTP in JVM mode, with the upstream test client.

The `ai/mcp` module covers the Quarkus annotations over Streamable HTTP, stdio and WebSocket, in JVM and native mode
and on OpenShift, with the LangChain4j MCP client, including sampling, elicitation, roots, logging and progress. It
has no standard-annotated features.

The gaps are:

- No tests over stdio and WebSocket, or in native and dev mode.
- No tests for applications that mix both annotation sets, or with multiple named server configurations.
- No tests of the extension interactions that are not part of the standard API, namely security, metrics and
  tracing.

## Planned coverage

All tests go into the existing `ai/mcp` module, reusing its server and client applications. The server application
gets new classes that declare features with the standard annotations.

The annotations and the protocol revision rarely affect each other. They only do so for a few attributes, such as
titles, icons and metadata, where whether the client sees the value depends on the revision in use. The revisions
themselves are covered by [QUARKUS-8895](QUARKUS-8895.md), so these tests simply run on whichever revisions the
module already uses and add no scenarios of their own for them.

- **Standard annotations**
  - Verify that the LangChain4j client lists and calls standard-annotated tools, prompts and resources over
    Streamable HTTP.
  - Verify that the listed titles, descriptions, tool annotations and arguments match the Quarkus-annotated
    equivalent, except for attributes the standard API does not define.
  - Verify the argument attributes the standard API does define: a renamed argument, an optional argument and an
    argument with a default value.
- **Mixed application**
  - Verify both annotation sets in one application and in one class, with standard-annotated tools using sampling,
    elicitation, roots and client logging, answered by the LangChain4j client.

## Impact on test suites and testing automation

No new modules or CI jobs.

Estimated 2 new test classes (standard annotations and mixed application), with about 8 test methods, and 2 new
classes in the server application.

## Impact on resources

The added tests should have **low impact** on resources:

* An estimated execution time increase of 1 to 2 minutes.
* Tests will be executed on baremetal in JVM and native mode, and on OpenShift.
* No new external services are required.

## Open questions

- A method annotated with both annotation sets is accepted without an error or warning: the Quarkus annotation is
  used and the standard one is ignored, whatever the declaration order. The intended behavior is not documented; to be
  raised upstream, since users migrating to the standard annotations may not notice that their declaration has no
  effect.

## Contacts

- Tester: Roberto Cortez <radcortez@ibm.com>

## References

- https://github.com/quarkiverse/quarkus-mcp-server/pull/869
- https://github.com/quarkiverse/quarkus-mcp-server/pull/1144
- https://github.com/quarkiverse/quarkus-mcp-server/issues/823
- https://github.com/mcp-java/java-mcp-annotations
- https://docs.quarkiverse.io/quarkus-mcp-server/2.0.x/index.html
- https://modelcontextprotocol.io/specification/2025-11-25
- https://modelcontextprotocol.io/specification/2026-07-28
- [QUARKUS-8895](QUARKUS-8895.md)
- [QUARKUS-7048](QUARKUS-7048.md)
- https://github.com/quarkus-qe/quarkus-test-plans/pull/336 (QUARKUS-8959)
