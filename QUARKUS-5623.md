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
- Log level sending config 
  - Log lines that are not displayed on other log channels (filtered due to too detailed log level), are not send to the backend as well. 
  Verify that sending to backend responds to changing log level config. 
- Bulk log sending - verify bulk size & time of sending. 
- Verify that hostname (part of log data send) is not fixed at build time, verify for both JVM and native.
- Verify that custom attributes can be sent using `quarkus.otel.resource.attributes`.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use
- Check the community status of the extension/feature, preview and experimental ones should be considered carefully

## Automated test development
- Test suite - New scenarios for baremetal and OCP execution.
- Test framework - requires implementation of new test service for grafana.

### Impact on TS and resources
- Two additional test classes in one module - for baremetal and OCP.
Native build will be only for one test on baremetal and one on OCP.
Expecting additional time to execute:
  - OCP JVM - 10 min
  - OCP native - 10 min
  - Baremetal JVM - 8 min
  - Baremetal native - 5 min

## Contacts
* Tester: Martin Očenáš <mocenas@redhat.com>
