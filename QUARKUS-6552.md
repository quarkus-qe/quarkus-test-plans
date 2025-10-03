# QUARKUS-6552 Add functionality to capture Quarkus application runtime data using JFR extension

JIRA: https://issues.redhat.com/browse/QUARKUS-6552

Add functionality to capture Quarkus application runtime data using JFR extension

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/48687

Documentation:
- https://quarkus.io/guides/jfr#runtime-event

## Scope of testing
This testing covers QUARKUS-6552, which adds an option to the `quarkus-jfr` extension to capture `QuarkusRuntime`, `QuarkusApplication`, and `Extension` JFR events.
The tests will check for the presence of these JFR events and their content within the `.jfr` file.


## Automated test development
The Quarkus JFR extension contains the unit tests that test the content of each mentioned JFR event.
These tests use `jdk.jfr.Recording` to record the events.
This triggers JFR events to start every time when the recording is started.
This is not reflection how the users of Quarkus will use this feature, as they will be expecting these events recorded on startup, without the need for manually trigger them.

Tests for recording of `QuarkusRuntime`, `QuarkusApplication`, and `Extension` JFR events without manual trigger will be implemented in Quarkus QE test suite.
The tests will check that all JFR events are recorded.
The content of these records will be checked and compared with expected values.

`QuarkusRuntime` JFR event should contain:
- Quarkus version
- Image mode (JVM/native)
- Profiles

`QuarkusApplication` JFR event should contain:
- Package name (`artifactId`) of the application
- Package version

`Extension` JFR event should contain:
- Name of extension

The `Extension` JFR event should be present multiple times, as each event contains one extension.
The tests will check if all extensions are present.


### Impact on TS and resources
A new `jfr` module will be created for these tests and will be part of the [`monitoring`](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/monitoring) module

The tests will be executed only on bare-metal for both JVM and native modes.
The expected time increase should be standard build time, plus roughly 30 seconds for test execution. 

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>