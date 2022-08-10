# QUARKUS-2082 - Run more of the QE test suite upstream

JIRA link: https://issues.redhat.com/browse/QUARKUS-2082

Goal: Run as many of the QE tests upstream, so that issues can be detected early, and eventual QE process runs more smoothly with less effort.

## Scope of the effort
- Analyze current coverage with Quarkus main on QE side
- Identify opportunities for improvements
- Implement automation for identified areas

## Current state
### QE testsuites + daily GitHub Actions
https://github.com/quarkus-qe/quarkus-test-framework
 - Daily Build 
 - Pull Request CI

https://github.com/quarkus-qe/quarkus-test-suite/
 - Daily Build
 - Pull Request CI
 - Quarkus ecosystem CI

https://github.com/quarkus-qe/quarkus-startstop
 - Daily Build
 - Pull Request CI

https://github.com/quarkus-qe/beefy-scenarios (used for RHBQ 1.x, now used for detection of changes in main)
 - Daily Build
 - Pull Request CI
 - Quarkus ecosystem CI

https://github.com/quarkus-qe/quarkus-openshift-test-suite (used for RHBQ 1.x, now used for detection of changes in main)
 - Pull Request CI

https://github.com/quarkus-qe/quarkus-extensions-combinations
 - Daily Build
 - Pull Request CI

https://github.com/quarkus-qe/quarkus-jdkspecifics
 - Daily Build
 - Pull Request CI

### PSI Jenkins + daily runs
 - Quarkus main build
 - Quickstarts development branch + Quarkus main
 - Database tests (JVM and native) from integration-tests module

## Opportunities
### QE testsuites
Weekly runs of QE test-suite against OpenShift (all modules) in PSI
 - Compilation and tests execution in JVM mode
 - Compilation and tests execution in Native mode
 - Execution against latest OCP version available in lab
 - Email based notification when execution fails

QE test-suite PR runs against OpenShift (affected modules) in PSI
 - Logic to determine affected modules by the changes
 - Compilation and tests execution of affected modules in JVM mode
 - Compilation and tests execution of affected modules in Native mode
 - Execution against latest OCP version available in lab
 - Notification on GH PR when execution fails

### Upstream CI
Run quarkus-quickstarts as part of Quarkus PRs
 - Compilation in JVM mode passes
 - Tests execution in JVM mode as optional step, depends on CI capacity
 - Native mode is covered through quarkus-ecosystem-ci

[Optional] Run quarkus-super-heroes as part of Quarkus PRs
 - Ccompilation and tests execution in JVM mode passes
 - Native mode as optional step

### OpenShift CI
 - OpenShift clusters ready to handle CI testing
 - Include OpenShift based testing into Quarkus main CI / PR runs

### Opportunities - upstream dev team
 - Windows instances
 - OpenShift cluster for experiments

## Coverage implementation
"QE testsuites" and "Upstream CI" topics to be implemented as part of this Feature Request.

"OpenShift CI" topic tracked in https://issues.redhat.com/browse/QUARKUS-2230

"Opportunities - upstream dev team" topics are not covered by https://issues.redhat.com/browse/QUARKUS-2082 as it focuses on CI related testing.

### Impact on test suites and testing automation
* Test automation to be developed
* No changes to the test suites are planned

### Impact on resources
* Weekly runs:
  * Capacity of 1 large + 1 xlarge executor for 8 hours
* PR runs:
  * Depends on PR size and frequency of PRs
  * Capacity of 1 large + 1 xlarge executor for 1-8 hours per PR
* Quickstarts:
  * One GH Action runner, needed for 10 minutes

## Future considerations
Topics identified in "Opportunities - upstream dev team" section
