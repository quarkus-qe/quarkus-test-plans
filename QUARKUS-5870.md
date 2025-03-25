# QUARKUS-5870 Websocket next

## Scope of testing
Verify quarkus' websocket-next extension.
Covers both websocket server and client.

### Test cases
- Smoke test - verify test client can connect to quarkus WS server and exchange messages.
  - Cover both text and binary messages.
- Multiple clients exchanging messages though server.
  - Verify broadcasting messages works for more than 2 clients.
- Exchange messages using reactive API.
- Cover telemetry gathering.
  - OpenTelemetry traces.
  - Micrometer metrics.
- Error handling - onError endpoint should be called if:
  - Runtime exception is thrown on endpoint
  - Uni/Multi receives a failure
- Test (de)serialization of object - with both default and custom codec.
- Ping, pong messages
  - Verify configuration for periodic ping sending - for both client and server.
  - Verify that pong messages are send and contain same data as ping.
- Concurrent processing mode - in this mode, quarkus does not guarantee ordering of events.
  - Test that long processing of one event, does not block the following one.
- Subwebsockets - verify these work and have correct HTTP path.
- Storing custom user data into sessions on server and client APIs.
- Inspecting HTTP upgrade to WS.
  - Verify the inspection event is called and upgrade can be rejected.
- TLS
  - Test a scenario with and without TLS. Cover both server and WS client config.
- Authentication
  - Annotation based.
  - Properties file based.
  - Using username & password or bearer token.
- Authorization
  - role-base access to endpoints.
  - permission checker.

Tests should cover cases where:
- Using WS client directly in tests
- Creating WS client in quarkus app

## Getting familiar with the feature
Getting familiar with
- Websocket next guide: https://quarkus.io/guides/websockets-next-tutorial
- Websocket next reference: https://quarkus.io/version/main/guides/websockets-next-reference

## Existing test coverage
QE TS already has tests for websocket-classic.
These will be used as a base for new tests.

### Impact on test suite
A new module for websocket-next will be created.

## Impact on resources
A new module will be created, requires additional test execution.
Tests will be executed on both JVM and native and on both baremetal and OCP.
Expected additional execution time in minutes for each case.

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>