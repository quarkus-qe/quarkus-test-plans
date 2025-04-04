# Quarkus-5857 WebSockets Next: Allow to send authorization headers from web browsers using JavaScript clients

Jira: https://issues.redhat.com/browse/QUARKUS-5857

Upstream PR: https://github.com/quarkusio/quarkus/pull/45809 that close these issues https://github.com/quarkusio/quarkus/issues/42824 and https://github.com/quarkusio/quarkus/issues/44409

## Scope of testing
Verifying the functionality that allows JavaScript WebSocket clients running in web browsers to send authorization headers to a Quarkus WebSocket Next server including testing over secure WebSocket connections (WSS).
This includes:
- Verify that web-browser-based JavaScript WebSocket clients can correctly format and send authorization (Bearer tokens) via the Sec-WebSocket-Protocol header
during the WebSocket handshake.
- Ensure that the Quarkus WebSocket Next server can receive, process, and validate the authorization information sent by web-browser clients for authentication 
and authorization purposes.
- Verify also that programmatic WebSocket clients (such as Vert.x WebSocketClient) can send authorization information in the expected format and that the server processes it correctly.
- Testing both positive and negative authorization scenarios using web-browser and programmatic clients, ensuring that missing, invalid, or unauthorized Bearer tokens result in connection rejection.

### Test cases
- Successful Authorization and Subprotocol Negotiation via Sec-WebSocket-Protocol header (WSS Web browser client - Automated with Playwright) - Verifies simultaneous authorization and subprotocol negotiation using the same header.
- Successful Authorization (after successfully authentication) via Sec-WebSocket-Protocol header (WS and WSS Web browser client - Automated with Playwright)
- Failed Authorization - Missing Sec-WebSocket-Protocol header (WSS Web browser client - Automated with Playwright)
- Failed Authentication - Invalid Bearer Token Format
- Failed Authorization - Unauthorized Bearer Token: Verifies that a client presenting a valid formatted Bearer token lacking the necessary roles or permissions to access a protected WebSocket endpoint or functionality is correctly denied access by the server.
- Successful Authorization (after successful authentication) via Sec-WebSocket-Protocol Header (Programmatic Client with Vert.x WebSocketClient)
- Failed Authorization - Missing Sec-WebSocket-Protocol Header (Programmatic Client with Vert.x WebSocketClient)
- Verify Correct Handling Post-Successful Authorization (WSS Web browser client - Automated with Playwright)

## Getting familiar with the feature
Getting familiar with
- WebSockets Next guide: https://quarkus.io/guides/websockets-next-tutorial
- WebSockets Next reference: https://quarkus.io/version/main/guides/websockets-next-reference

## Existing test coverage
QE TS already has tests for quarkus-websocket-next introduced by QUARKUS-5870.md.

### Impact on test suite
New classes and tests will be created into 'quarkus-test-suite/websockets/websocket-next-oidc'.

## Impact on resources
Tests will be executed on both JVM and native and on both baremetal and OCP.
Expected additional execution time in minutes for each case.

## Contacts
* Tester: Jose Carranza <jcarranz@redhat.com>