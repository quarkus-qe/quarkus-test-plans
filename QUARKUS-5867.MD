# QUARKUS-5867 - Micrometer to OpenTelemetry Bridge

JIRA: https://issues.redhat.com/browse/QUARKUS-5867

Upstream issue: https://github.com/quarkusio/quarkus/issues/43621

Upstream PR: https://github.com/quarkusio/quarkus/pull/43831

Quarkus 3.19 introduced a new Quarkus Micrometer to OpenTelemetry Bridge extension with a tech preview status.
The extension integrates the `io.opentelemetry.instrumentation:opentelemetry-micrometer-1.5` library
with the Quarkus OpenTelemetry and Quarkus Micrometer extensions, thus allowing to support both Micrometer and OpenTelemetry
in one Quarkus application, without the need to export the metrics into two collectors.
The Micrometer semantic convention is not compatible with the OpenTelemetry semantic convention; therefore,
Micrometer metrics are mapped to the OpenTelemetry metrics.

## Scope of the testing

New test coverage should verify that:

- all telemetry data (logs, tracing, metrics) created with both Micrometer and OpenTelemetry processed by the same OpenTelemetry SDK instance, and will use its configuration and exporters
- Micrometer-specific configuration is possible even when using the bridge extension, for example, the following must work:
    - enabling individual binders (HTTP server, HTTP client, Vert.X, ...)
    - exporting `DistributionSummary`
- Both automatically generated Micrometer metrics and custom-created metrics are correctly recorded and exported with the proper names, tags, and values
- Differences in the semantic conventions work exactly as explained in the [Micrometer and OpenTelemetry extension guide](https://quarkus.io/guides/telemetry-micrometer-to-opentelemetry)
- [Observability Dev Services](https://quarkus.io/guides/observability-devservices) can be used to develop a project with the bridge extension
- Works in JVM, native, and DEV mode
- Application is deployed and runs in OpenShift without related issues

## Existing test coverage

Quarkus QE tests Micrometer and OpenTelemetry extensions separately; there is no test coverage for this new extension.

Quarkus upstream JVM test coverage is excellent; however, there is no test covering a native mode or a DEV mode,
and the JVM mode tests are running in the same Java process (they are not testing JARs).
OpenShift tests are missing as well.

### Impact on test suites and testing automation

A new `monitoring/micrometer-opentelemetry` module will be added to the Quarkus QE Test Suite with two dedicated test classes for each environment and mode.

### Impact on resources

Overall test execution time will increase roughly by 35 minutes:

- 5 minutes for JVM mode
- 10 minutes for native mode
- 5 minutes for DEV mode
- 10 minutes for the OpenShift environment and JVM mode
- 15 minutes for the OpenShift environment and native mode

## Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Quarkus - Add support for OpenTelemetry metrics](https://github.com/quarkusio/quarkus/issues/39033)
- [Micrometer Instrumentation for Micrometer version 1.5 and higher](https://github.com/open-telemetry/opentelemetry-java-instrumentation/tree/main/instrumentation/micrometer/micrometer-1.5/library)
