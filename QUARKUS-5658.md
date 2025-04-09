# QUARKUS-5658: Integrate OpenTelemetry to the WebSockets Next extension

Jira: https://issues.redhat.com/browse/QUARKUS-5658

This feature is specifically OpenTelemetry integration part of the new WebSockets Next extension.

## Scope of the testing

### Upstream testing
* Unit tests have been added with implementation of this integration. Those 
  unit tests verify that the correct spans are created on opening and closing
  of the websocket connection.

### General
* An integration test will be added to Quarkus QE test suite's `monitoring`
  module. This test will verify that spans are created on connection open.
  Test application will run both a websocket server and a client to verify
  spans are created for both.

### Impact on test suites and testing automation
* Single test case will be added to `monitoring` module for both bare metal
  and OpenShift.

### Impact on resources
* New test should be executed within a minute or single minutes for both bare
  metal and OpenShift.

## References
* Feature epic: [QUARKUS-5658: Integrate OpenTelemetry to the WebSockets Next extension](https://issues.redhat.com/browse/QUARKUS-5658)
* Websockets.next test plan: [QUARKUS-5870](QUARKUS-5870.md)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>