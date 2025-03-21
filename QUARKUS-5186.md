# QUARKUS-4573 - Enable Quarkus QE baremetal test execution of QuarkusIO/Quarkus project test coverage against RHBQ builds

JIRA: https://issues.redhat.com/browse/QUARKUS-5186

TD:
- internal repo (Red Hat GitLab with Jenkins Jobs mainly)
- https://github.com/quarkus-qe/quarkus-extracted-tests
- https://github.com/quarkus-qe/quarkus-test-extractor

[Quarkus main project](https://github.com/quarkusio/quarkus) has automated unit and integration test coverage that should verify functional and other requirements.
Quarkus QE is tasked with Red Hat Build of Quarkus verification, which from practical POV means we need to have capability to run tests against delivered RHBQ artifacts.
So far our team has tested the product with our own test coverage, however the Quarkus main project should be more exhaustive, and it has additional advantages, like that it is maintained by engineering teams.
The goal of this task is to extend the test coverage used for RHBQ verification by executing the Quarkus main project tests as well.

## Scope of the testing
Run bare-metal unit and integration tests created by the Quarkus main project contributors as part of general trigger pipeline used for RHBQ verification.
We will run unit tests and integration tests in JVM mode and integrations tests in native mode.

## Future considerations

- Run adapted or dedicated integration tests created by the Quarkus main project contributors in OpenShift
- Run tests on Windows
- Run tests on Aarch64
- Run tests with Podman instead of Docker

## Existing test coverage
So far upstream test coverage is run as part of the Quarkus main project PR CI.
Test are not run with the RHBQ at all, but we verify selected scenarios with _upstream test coverage_ based on the CI results.

### Impact on test suites and testing automation
New Jenkins jobs will be added to RBHQ 3.15 and 3.20 general test triggers.
No immediate impact on the Quarkus QE Test Suite, but it will give as a chance to contribute tests that don't have E2E nature, or don't test many extensions together (logging, tracing, security, REST, ...) upstream.
Depending on selected strategy, it could have positive impact on our TS.

### Impact on resources
Test execution time will increase by more than 20 hours.
There is no precise number, for number of tests is increasing every single day and the time depends on test runners as well.
We will create a matrix Jenkins job with 30 scenarios.
Each scenario will consist of `total number of modules / 30` test modules.
The `30` magical number will be manually configurable and can be adapted on the targeted test execution time.
We will group extensions tests together, as we will do with integration tests.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>
