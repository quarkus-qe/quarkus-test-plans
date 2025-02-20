# QUARKUS-5656 OTel metrics for MicroProfile Telemetry 2.0

JIRA: https://issues.redhat.com/browse/QUARKUS-5656

The upstream work aims to provide compliance with OpenTelemetry metrics according to https://github.com/eclipse/microprofile-telemetry/blob/2.0/spec/src/main/asciidoc/metrics.adoc.

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/43678

## Scope of the testing
QE team will:
- verify MicroProfile Telemetry 2.0 compliance was achieved
- ensure there are mechanisms in place to detect regressions through MicroProfile Telemetry 2.0 TCK
- ensure https://quarkus.io/standards/ page reflects compliance with MicroProfile Telemetry 2.0
- add test coverage in QE TS for sanity checks
  - MicroProfile Telemetry specific metrics, e.g. CPU
  - JVM and HTTP server metrics configuration options

## Getting familiar with the feature
Getting familiar with
- https://quarkus.io/guides/opentelemetry-metrics
- https://github.com/microprofile/microprofile-telemetry/blob/2.0/spec/src/main/asciidoc/metrics.adoc

## Existing test coverage
QE TS coverage will be enhanced to have sanity checks in place.

### Impact on test suites and testing automation
The existing module will be enhanced with additional scenarios.

### Impact on resources
The expected execution time will increase in the range of seconds for JVM mode. Native mode can be affected in a range of minutes
if a new native binary will be built, otherwise the range of seconds.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## References
- https://quarkus.io/guides/opentelemetry-metrics
- https://github.com/microprofile/microprofile-telemetry/blob/2.0/spec/src/main/asciidoc/metrics.adoc
- https://github.com/microprofile/microprofile-telemetry/blob/main/tck/tracing/README.adoc#declaring-the-tests-to-run