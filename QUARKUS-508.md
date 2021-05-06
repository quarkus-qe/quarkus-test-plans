# QUARKUS-508 Create one or more quarkus devfile v2, ensure odo based workflow

Jira: https://issues.redhat.com/browse/QUARKUS-508

The goal of QUARKUS-508 is to provide a runtime descriptor for OpenShift development tools (e.g. odo, Eclipse Che).
For purposes of this feature request, odo based workflow will be considered.

## Scope of the testing

- Define the scope of the testing
- Analyze the possible impact on Quarkus itself - e.g performance, compatibility
- Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
- Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources
- Determine the ownership and scope of testing of the default devfile registry
- Determine the ownership and scope of testing of `quay.io/eclipse/che-quarkus:nightly` container that is used as base
  container for builds produced by this devfile

### Impact on testsuites and testing automation:
A test for starter application created and deployed by the `java-quarkus` component devfile will be implemented. This 
test will be ran in a separate, new job, verifying that the `redhat-product` starter application defined in the 
`devfile` produces the correct productised starter application.

A deployment strategy utilising `odo` tool will be implemented in [`quarkus-test-framework`](https://github.com/quarkus-qe/quarkus-test-framework/).

A test using the `odo` deployment strategy will deploy a simple REST endpoint and verify that the endpoint responds.
This test will be implemented in [`quarkus-test-suite`](https://github.com/quarkus-qe/quarkus-test-suite/) project.

### Impact on resources:
A new job testing the starter application defined in the `java-quarkus` component devfile will be implemented and ran 
in the standalone. This execution should be completed within minutes.

The new test implemented in Quarkus OpenShift test suite does not require infrastructural changes and its execution
should be finished within minutes. Test will be executed on the latest released OCP.

The used jobs will manage `odo` installation and cleanup.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- `odo` tool study
- `devfile` v2 study
- testing the `odo` based workflow on quickstarts manually
- check for the possibility of changing devfile repository to allow for testing before GA

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* 
* Runtimes epic: [RHMWRT-364 DevFiles v2 : providing Runtime "Stacks" for OpenShift dev tools (odo/IDE/Che/DevConsole)](https://issues.redhat.com/browse/RHMWRT-364)
