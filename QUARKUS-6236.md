# QUARKUS-6236: Load test classes with runtime classloader

Jira: https://issues.redhat.com/browse/QUARKUS-6236

Code changes:
* https://github.com/quarkusio/quarkus/pull/34681
* https://github.com/quarkusio/quarkus/pull/46780
* https://github.com/quarkusio/quarkus/pull/46391
* https://github.com/quarkusio/quarkus/pull/47190
* https://github.com/quarkusio/quarkus/pull/41621

This story is about changing architecture so that the classloader used to load
test classes is the same one that executes them. 

Previously, tests were loaded by the classloader of Quarkus application being
tested. The implementation required serialization that had issues with running
past Java 17, and the way the tests were loaded, incorrect versions of tests
appeared on the application runtime classloader. For more information, see the
linked issues at https://github.com/quarkusio/quarkus/pull/34681.

As this is an architectural change, not a user facing feature, the focus of
testing will be ensuring there are no regressions or unknown breaking changes.

## Scope of the testing

QE will
* get familiar with the changes
* assess test coverage provided as part of the code changes
* monitor for test failures in Quarkus QE test suite
* report regressions and breaking changes

### Upstream testing
* Pull request for the architecture change contains test updates to account for
  the changes introduced.
* Tests for service loader adding QuarkusTestExtension were added with 
  https://github.com/quarkusio/quarkus/pull/46780
* With https://github.com/quarkusio/quarkus/pull/46391, further tests for
  more complex scenarios (multiple profiles, reverts) were added for continuous
  testing

### General
* Existing test coverage in Quarkus QE test suite will be used for regression
  testing on all platforms with accordance to release stream test plan.

### Impact on test suites and testing automation
* No impact, as this is about regression testing

### Impact on resources
* No impact, as this is about regression testing

## References
* Feature story: [QUARKUS-6236: Load test classes with runtime classloader](https://issues.redhat.com/browse/QUARKUS-6236)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>