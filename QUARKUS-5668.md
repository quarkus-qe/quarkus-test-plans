# QUARKUS-5667: Integrate Micrometer with WebSockets Next

Jira: https://issues.redhat.com/browse/QUARKUS-5668

Upstream documentation: [Disabling Specific Traces for Application Endpoints](https://quarkus.io/guides/opentelemetry-tracing#disabling-traces-for-app-endpoints)

Code changes: 
* https://github.com/quarkusio/quarkus/pull/43885
* https://github.com/quarkusio/quarkus/pull/44724

This feature request covers a configuration item covering paths that should be
excluded from tracing. Original implementation also included `@Traceless` 
annotation (both class and method scoped), however support for this has been
removed with later Quarkus versions due to the fact that REST endpoints do not
have one-to-one mapping to paths.

## Scope of the testing

### Upstream testing
* A unit test has been added verifying that only desired paths produce traces.

### General
* A new test will be implemented in `monitoring/opentelemetry-reactive` module.
  This test will deploy an application with defined `quarkus.otel.traces.suppress-application-uris`
  property and then assert no traces will be produced for paths configured in
  this entry.
  * This test should also check sanity of behaviour of this filtering with 
   `@PathParam` and `@QueryParam`.

### Impact on test suites and testing automation
* Single test case will be added to `monitoring/opentelemetry-reactive` module
  for both bare metal and OpenShift. Both of those scenarios will run in both
  the JVM and native mode.

### Impact on resources
* New test should be executed within a minute or single minutes for both bare
  metal and OpenShift.

## References
* Feature epic: [QUARKUS-5668: Add ability to exclude URIs from OpenTelemetry Tracing](https://issues.redhat.com/browse/QUARKUS-5668)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>