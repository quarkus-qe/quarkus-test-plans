# QUARKUS-1396 - Assisted updates

JIRA link: https://issues.redhat.com/browse/QUARKUS-1396

RHBQ users need to be aware of the new versions available to do the update.
This RFE provides tooling to help the users to use the latest available version.

Focus of this RFE is on quarkus maven plugin.

There are plans to provide the functionality in CLI and enhancements to report differences between project and product.
These aspects are not covered by this RFE.

## Scope of the testing
- Check availability of `mvn quarkus:info` command
- Check availability of `mvn quarkus:update` command
- Generate project using older Quarkus version
    - Check output of `mvn quarkus:info` command
    - Check output of `mvn quarkus:update` command
- Generate project using older RHBQ version
    - Check output of `mvn quarkus:info` command
    - Check output of `mvn quarkus:update` command
- Focus on exploratory testing
- Automating base checks, more scenarios to be based on CLI use-cases

### Impact on testsuites and testing automation:
- quarkus-startstop test suite needs to be extended to cover execution of `mvn quarkus:abc` goals
- quarkus-startstop test suite needs to be extended to cover tests for `quarkus:info` and `quarkus:update` availability
- quarkus-test-framework and quarkus-test-suite are not desired test development target as they are focused on running applications
- quarkus-test-suite will be used for test development once Quarkus CLI gets enhanced with `info` and `update` commands

### Impact on resources:
- No additional requirements for resources in lab
- Prolonged execution time by ~1 minute

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the features
- Explore various applications with different sets of extensions

## Automated test development
This step should ensure:
- quarkus-startstop test suite based test development

## Advanced topics for test development
N/A