# QUARKUS-2812 Build time metrics for Quarkus

JIRA link: https://issues.redhat.com/browse/QUARKUS-2812.

The aim of this feature is to collect build-time data in a Quarkus application and send them to an external data platform
(segment.io) for analytical purposes.
The application collects various data (see bellow) during the build, logs them and sends them to the 3rd party service.
It also logs the time the entire metrics-related operation takes.
The functionality can be deactivated by:
- Quarkus configuration property.
- Static list of application artifact group ID patterns (`io.quarkus*`, `io.quarkiverse`, `org.acme`, `org.test`).
- Dynamic remote config.
  - Retrieved from a remote endpoint, locally cached (`{user.home}/.redhat/`). The local cache has a refresh interval.
  - Config properties, which may be used to disable the metrics:
    - All on/off.
    - Deny for a list of user IDs.
    - Deny for a list of Quarkus versions.

Build failure prevents the metrics to be processed.
Failure in the metrics collection (i.e. a bug in Quarkus) causes the build to fail.
Failure in the metrics sendoff (network issue, endpoint unavailability, ...) does NOT cause the build to fail.

## Scope of testing

What data is collected in what event.
- In all events, identical set of data is being collected.
- Events:
  - Dev mode build.
  - Production build (JVM, native).
- Data:
  - Build type.
  - Quarkus version, Java vendor and version, GraalVM/Mandrel version, OS name and version, build tool and its version (Gradle, Maven).
  - Anonymous User ID, anonymous Project ID.
  - List of extensions and their versions.
    - Only extensions with defined group IDs should be collected (`io.quarkus*`, `io.quarkiverse*`).
  - Build duration, build diagnostics (collection of events which are being logged during Quarkus build).

Verify the data being logged during application build.
- Do not query the remote 3rd party service, which could prove troublesome.

Verify, that no sensitive data / personal identifiable information is being shared (e.g. IP address).

Verify all the mechanisms to disable the metrics work properly.
- Quarkus config property.
- Static list of group IDs.
- Remote config.
  - Provide a cached config and verify that it is being used.
  - Provide an expired cached config and verify that it is being refreshed.

Verify that a clear notice about the telemetry and the nature of the collected data is part of Quarkus release.
- Documentation.
- Log message about the telemetry being active/inactive.

## Getting familiar with the feature
Exploratory testing:
- Verify application build scenario with the data platform service unavailable / no internet connection.

User experience:
- Verify the documentation.
- Verify clear notice on the data collection.

## Automated test development

`Quarkus QE TS` is a suitable place for adding the tests - new module `buildtime-analytics`.
- Make use of Quarkus CLI (`io.quarkus.qe:quarkus-test-cli`) similarly to `quarkus-cli` module.
  - Create and build applications.
  - Inspect applications' logs and verify that they contain expected metrics data.
- Various application setups.
  - Different sets of extensions.
  - Different ways of disabling the metrics.
- Various build types: dev, JVM, native.
- Build failure scenario.
- Check impact on build time - processing time the build log can be compared to a predefined threshold.

## Advanced topics for test development

Implement equivalent scenarios for Gradle.
- For the future, not part of planned test development, since Gradle is currently not supported on product side.

## Impact on test suites and test environment

Number of new scenarios:
- 8 in JVM mode: (extensionsSetA (small), extensionsSetB (large)) x (dev mode, prod mode) + 4 scenarios with disabled metrics.
  - ~1 minute of build time each
- 2 in native mode: extensionsSetA (small), extensionsSetB (large).
  - ~5 minutes of build time each.
