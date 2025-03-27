# QUARKUS-5186 - Enable Quarkus QE baremetal test execution of QuarkusIO/Quarkus project test coverage against RHBQ builds

JIRA: https://issues.redhat.com/browse/QUARKUS-5186

TD:
- internal repo (Red Hat GitLab with Jenkins Jobs mainly)
- https://github.com/quarkus-qe/quarkus-extracted-tests
- https://github.com/quarkus-qe/quarkus-test-extractor

[Quarkus main project](https://github.com/quarkusio/quarkus) has automated unit and integration test coverage that should verify functional and other requirements.
Quarkus QE is tasked with Red Hat Build of Quarkus verification, which from practical POV means we need to have capability to run tests against delivered RHBQ artifacts.
So far our team has tested the product with our own test coverage, however the Quarkus main project should be more exhaustive, and it has additional advantages, like that it is maintained by engineering teams.

Delivered RHBQ bits should be 99.99 % same, therefore the fact that we only executed our tests were sufficient to detect a production process issues that would cause difference between community and product builds.
The goal of this task is to extend the test coverage used for RHBQ verification by executing the Quarkus main project tests as well.
The capability run these tests with RHBQ will avoid test development duplications on the QE side for tests run on bare-metal, cleanup Quarkus QE Test Suite, thus decrease maintenance and test execution time.

We wish to follow with adding a capability to run the Quarkus main project tests in OpenShift and elaborate with engineering team on test requirements for noteworthy features 
(for example in [Working Groups](https://quarkus.io/working-groups/)) as well as on significant bugs, which would allow further Quarkus QE Test Suite cleanup, because
the test development will already be fully covered by the Quarkus Main project tests.

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
New Jenkins jobs will be added to 3.20 general test trigger.
Once Quarkus QE can rely on the Quarkus main project bare-metal and cloud-native test coverage, we will clean up Quarkus QE Test Suite from tests that merely duplicate upstream test coverage.
The cleanup will decrease time required for a test execution and test maintenance as we will pass the burden to our development team.

### Impact on resources
Test execution time will increase by more than 20 hours.
There is no precise number, for number of tests is increasing every single day and the time depends on test runners as well.
We will create a matrix Jenkins job with 30 scenarios.
Each scenario will consist of `total number of modules / 30` test modules.
The `30` magical number will be manually configurable and can be adapted on the targeted test execution time.
We will group extensions tests together, as we will do with integration tests.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>
