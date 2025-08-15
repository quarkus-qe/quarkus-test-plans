# QUARKUS-6247 Dev UI: Added basic workspace feature

JIRA: https://issues.redhat.com/browse/QUARKUS-6247

Upstream PR: https://github.com/quarkusio/quarkus/pull/46907

New Workspace feature was added to Quarkus Dev UI. It allows users to open and edit project files.

## Scope of the testing

Opening and editing files in the workspace. The changes to files should reflect in dev mode.

### Out of scope

Custom actions are out-of-scope.

## Existing test coverage

There is no coverage upstream and just minimal coverage of DevUI in our Test suite.

### Impact on test suites and testing automation

This plan presumes, that it is possible to test the behavior of Dev UI in Quarkus test suite. If thsi assumption is correct:
1. New tests(or tests classes) would be added into http-minimum module.
2. Tests will be executed on JVM in Dev Mode.

Otherwise:
The verification will have to be done manually.

### Impact on resources
Test execution time should increase in a couple of minutes.

## Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Fedor Dudinsky <fdudinsk@redhat.com>

## References
- https://quarkus.io/guides/dev-ui#workspace
