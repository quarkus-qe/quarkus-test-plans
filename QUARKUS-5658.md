# QUARKUS-5658: Integrate OpenTelemetry to the WebSockets Next extension

Jira: https://issues.redhat.com/browse/QUARKUS-5658

Upstream documentation: [Websockets next reference guide - Telemetry](https://quarkus.io/version/main/guides/websockets-next-reference#telemetry)

Code changes: https://github.com/quarkusio/quarkus/pull/41956

This feature is specifically OpenTelemetry integration part of the new WebSockets Next extension.

## Scope of the testing

### Upstream testing
* Unit tests have been added with implementation of this integration. Those 
  unit tests verify that the correct spans are created on opening and closing
  of the websocket connection.

### General
* An integration test will be added to verify that traces are created for both
  of the websocket operations (open and close) and that the close connection 
  trace correctly refers to the open connection trace.

### Impact on test suites and testing automation
* Single test case will be added to `websockets/websocket-next` module for both
  bare metal and OpenShift. This test will be ran in both the JVM and native 
  mode in both scenarios.

### Impact on resources
* New test should be executed within a minute or single minutes for both bare
  metal and OpenShift.

## References
* Feature epic: [QUARKUS-5658: Integrate OpenTelemetry to the WebSockets Next extension](https://issues.redhat.com/browse/QUARKUS-5658)
* Websockets.next test plan: [QUARKUS-5870](QUARKUS-5870.md)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>