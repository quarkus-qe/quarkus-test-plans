# QUARKUS-5623 - Logging in OpenTelemetry

JIRA: https://issues.redhat.com/browse/QUARKUS-5623

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/38239
- https://github.com/quarkusio/quarkus/pull/46354

Documentation: https://quarkus.io/version/main/guides/opentelemetry-logging

## Preview status
OpenTelemetry logging is currently considered dev-preview in quarkus.

## Scope of testing
- Test OpenTelemetry quarkus extension.
- Verify that quarkus application is sending its logs to the logging backend.
  - Example backend uses grafana/otel-lgtm, which is all-in-one solution using grafana for frontend and Loki for log collecting. 
- Verify that log messages as well as log levels are correct.
  - Log formatting is not tested as log lines store individual fields for log level, message, time, etc.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use
- Check the community status of the extension/feature, preview and experimental ones should be considered carefully

## Automated test development
- Test suite - New scenarios for baremetal and OCP execution.
- Test framework - requires implementation of new test service for grafana.

### Impact on TS and resources
- Two additional test classes in one module - for baremetal and OCP. Expecting additional time to execute:
  - OCP JVM - 2 min 30 seconds
  - OCP native - 10 min
  - Baremetal JVM - 2 min
  - Baremetal native - 5 min

## Contacts
* Tester: Martin Očenáš <mocenas@redhat.com>
