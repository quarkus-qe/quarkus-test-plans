# QUARKUS-7045 Add support for Java 25

JIRA: https://issues.redhat.com/browse/QUARKUS-7045

Add support for Java 25

## Scope of testing

The main focus of this test plan is on the product side, as most tests in upstream repositories are already running with Java 25.
There is a Quarkus upstream working group https://github.com/orgs/quarkusio/projects/59 that tracks all efforts.
Since not all issues/PRs are closed, it is possible that running the Quarkus application may trigger some warnings originating from external libraries.


### Current test coverage
* Quarkus upstream runs the tests with Java 25 (Semeru and Temurin)
* QE testsuite and framework run the daily build of main with Java 25 (Temurin)


### Impact on test suites and testing automation
The Java 25 (OpenJDK, Temurin, and Semeru) will be added to Quarkus QE Jenkins.
The Temurin 25 will be added in Windows recipe for execution on AWS.

For JVM:
* Bare metal Linux jobs in the extended platform will be extended to include all of the Java 25 distributions mentioned.
* In addition to Temurin 17 and 21, Temurin 25 will also run on AWS on bare metal.
* OpenShift (`openshift-ts-jvm-ocp` job) will be extended with Java 25. This job execute only selected modules.
* Aarch64 bare metal will not be extended to Java 25 because RHEL 8 does not include the Java 25 package in its repository.
* Aarch64 OpenShift will be extended with Java 25 and will run alongside Java 17 and 21.

For Native:
* The new Mandrel based on Java 25 will be used.
* JDK 21 mandrel won't be tested for 3.32+ streams. The only exception is ubi8 compatibility testing, which will use `quay.io/quarkus/ubi-quarkus-mandrel-builder-image:jdk-21` (same as for 3.27 stream).
* Native jobs will use runners that set Java 25 as its JDK.
* Aarch64 will use Mandrel based on Java 25, and runners will use Java 21, due to the limitation mentioned in the JVM section.

In addition to this there will be job in general testing pipeline for verification of [QUARKUS-6969](https://issues.redhat.com/browse/QUARKUS-6969) for each .z release.
The extending the Jenkins with this job is described in [QUARKUS-6969 TP](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6969.md#automated-test-development).


### Impact on resources
The impact will be some additional jobs running Java 25.
For native, the number of tasks remains the same, as Java 21 has been replaced by version 25.
Five additional jobs will be added for Linux JVM, and two tasks will be added and one removed for Windows.
There are impacted jobs which are defined by a matrix.
These jobs will be updated to minimize resources, but more runners will still be needed because the test matrix will consist of three versions of Java.

The biggest impact on resources will be bare metal testsuite jvm matrix job, as Java 17 and Java 21 require 90 large Linux runners.
The addition of Java 25 will increase the number of runners by 30 to 120.

### Impact on testing cycle
As Java 25 increase execution time, the CR build testing will be needed to extend testing window by one day.

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>

## References
* https://issues.redhat.com/browse/QUARKUS-7045
* https://issues.redhat.com/browse/QUARKUS-6969
* https://github.com/orgs/quarkusio/projects/59