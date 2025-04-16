# QUARKUS-5667: Integrate Micrometer with WebSockets Next 

Jira: https://issues.redhat.com/browse/QUARKUS-5667

Upstream documentation: [Websockets next reference guide - Telemetry](https://quarkus.io/version/main/guides/websockets-next-reference#telemetry)

Code changes: https://github.com/quarkusio/quarkus/pull/44379

This feature is specifically Micrometer integration part of the new WebSockets Next extension.

## Scope of the testing

### Upstream testing
* Unit tests have been added with implementation of this integration. Those
  unit tests verify that the instrumentation produces correct values for 
  the following gauges and counters for both client and server:
  * connection error counter
  * connection counter
  * endpoint error counter
  * inbound/outbound messages counter
  * inbound/outbound bytes counter
  * connections closed counter

### General
* An integration test will be added to Quarkus QE test suite's 
 `websockets/websocket-next` module, executed in both JVM and native mode. This
  test will verify that the following metrics are correctly produced for both 
  the websockets next client and server:
    * connection error counter
    * connection counter
    * endpoint error counter
    * inbound/outbound messages counter
    * inbound/outbound bytes counter
    * connections closed counter

### Impact on test suites and testing automation
* Single test case will be added to `monitoring` module for both bare metal
  and OpenShift.

### Impact on resources
* New test should be executed within a minute or single minutes for both bare
  metal and OpenShift.

## References
* Feature epic: [QUARKUS-5667: Integrate Micrometer with WebSockets Next](https://issues.redhat.com/browse/QUARKUS-5667)
* Websockets.next test plan: [QUARKUS-5870](QUARKUS-5870.md)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>