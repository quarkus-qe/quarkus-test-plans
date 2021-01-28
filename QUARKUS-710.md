# QUARKUS-710 - Add Support for Micrometer

JIRA link: https://issues.redhat.com/browse/QUARKUS-710

Micrometer is a metrics instrumentation library for JVM-based applications. It provides a simple facade over the instrumentation clients for the most popular monitoring systems. Currently, the testing will only scope the integration with Prometheus.

For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing

Following actions were taken to ensure test coverage and automation for native image:
 - Define the scope of the testing
 - Analyze the possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
 - Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources

 As part of the development, the following test coverage has been implemented:
- Support for the MicroProfile Metrics API (`integration-tests/micrometer-mp-metrics`)
- Scenario with custom tags using `@MeterFilterConstraint` annotation (`integration-tests/micrometer-prometheus`)
- Scenario to use a custom Prometheus registry (`integration-tests/micrometer-prometheus`)

### Impact on testsuites and testing automation:
 - OpenShift TS framework needs to be able to use the embedded Prometheus instance in OpenShift
 - Smoke tests to be added into the OpenShift TS:
   - Verify custom Application metrics are shared between the Quarkus application and Prometheus
   - Verify collecting Metrics from a Kafka instance - QUARKUS-726
 - More test development needs to be added in Beefy-scenarios testsuite:
   - Scenario with parallel processing of metrics
   - Scenario to cover the correct functionality of the `vertx` related properties
 - Ensure this coverage works both in JVM and NATIVE mode

### Impact on resources:
 - More services will be running on OpenShift cluster, impact on CPU/memory should be minimal
 - Test execution will be prolonged, preliminarily estimation is ~30 minutes per each OCP stream
 - QUARKUS-710 covers both JVM and NATIVE mode, Native image imposes multiple times prolonged execution time and increased memory requirements for the build

Our current testing capacity is sufficient to cover this testing.

If multiple streams get introduced we need to have increased capacity in the lab.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage