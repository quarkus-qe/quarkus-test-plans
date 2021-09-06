## QUARKUS-280 - Tech Preview support for Message tracing and Open Telemetry

JIRA link: https://issues.redhat.com/browse/QUARKUS-280  
Analysis link: <https://issues.redhat.com/browse/QUARKUS-280?focusedCommentId=18936836&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-18936836>

OpenTelemetry extension for Quarkus is a preferred future solution for the application's telemetry. The current OpenTracing library for telemetry from Smallrye is deprecated and will not be supported in the future.
Quarkus brings three extenions related to OpenTelemetry:
 - quarkus-opentelemetry
 - quarkus-opentelemetry-exporter-jaeger
 - quarkus-opentelemetry-exporter-otlp

 This feature request is supposed to focus primarily on quarkus-opentelemetry and quarkus-opentelemetry-exporter-jaeger, according to PM they are the demanded by other products.

 OpenTelemetry is used to collect the application's telemetry data (metrics, logs, and traces) and to track how a particular user request is traversing through the different stages in a distributed system. Collected data are used to determine application performance and to investigate its behavior.

 Quarkus Guide: https://quarkus.io/guides/opentelemetry  
 Quarkus Guide upstream: https://github.com/quarkusio/quarkus/commits/main/docs/src/main/asciidoc/opentelemetry.adoc

## Scope of the testing
 - New tests in QE TS to try OpenTelemetry on OpenShift
 - Reach equivalent test coverage as for OpenTracing in QE TS

For the detailed analysis of the existing upstream coverage see the *Analysis link*.

### Impact on testsuites and testing automation:
- Test will be implemented in https://github.com/quarkus-qe/quarkus-test-suite

### Impact on resources:
 - 1 OpenTelemetry Collector (~100 MB)
 - 1 Jaeger Backend (~60 MB)

## Getting familiar with the feature
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use