# QUARKUS-3007: Aarch64/ARM Support for both native and JVM Quarkus apps

Jira: https://issues.redhat.com/browse/QUARKUS-3007

The goal of this feature is to provide production support in the same scope for aarch64 architecture that RHBQ provides
for x86-64 architecture.

For the purposes of planning, the following tasks therefore need to be executed (ordered by priority):
* Same test coverage and certification matrix on OCP on aarch64 for both JVM and native mode as on OCP on x86-64 (see
  https://issues.redhat.com/browse/QUARKUS-3446)
* Certification of RHBQ native mode on OCP on aarch64
* Certification of RHBQ in both JVM and native mode in the bare metal scenarios on aarch64

## Scope of the testing

### General
* Catch-up on the test coverage on OCP to be same for aarch64 as it is for x86-64
  * Enabling Serverless tests in `openshift-arm` profile once RH Serverless supports aarch64
  * Running test services that do not yet support aarch64 in a side OpenShift cluster running on x86-64
    * This pertains to units to tens of tests across multiple modules 
* Certification of RHBQ native mode on OCP on aarch64
  * The full native OpenShift coverage will have to be ran in native mode with the Mandrel native builder container
* Certification of both the JVM and native modes in bare metal scenarios will require all the aspects to be tested
  * we support two JDK versions for JVM mode and one JDK version for native mode per release stream
  * start-stop TS with RHBQ code starters
  * start-stop special characters TS
  * bare metal test suite
  * Quarkus Quickstarts

### Impact on test suites and testing automation
* Catch-up on the test coverage on OCP to be same for aarch64 as it is for x86-64
  * This means that the aarch64 OCP testing pipeline would require an additional x86-64 cluster to be deployed in AWS
    and test services be prepared before the test execution by the test pipeline. The other part of this is that the
    tests that currently use test services not supported on aarch64 would be moved to standalone test suite module, only
    executed in `openshift-arm` profile
  * The other aspect is testing on EUS OpenShift. A job executing the OpenShift interoperability modules on a matrix of 
    JDK version and OpenShift version should be implemented.
  * We will also need to implement periodic runs of OpenShift test suite in JVM mode with Quarkus built from main branch
    on aarch64. 
* Certification of RHBQ native mode on OCP on aarch64
  * We need machines that execute the OpenShift test suite to be aarch64 too, as we need to compile the native binaries
    on the same architecture they're deployed to.
  * Pipeline will need to include reservation of aarch64 systems from Beaker, their provisioning and addition to
    Jenkins besides the execution of the test coverage.
  * We will also need to implement periodic runs of OpenShift test suite in native mode with Quarkus built from main 
    branch on aarch64.
* Certification of both the JVM and native modes in bare metal scenarios on aarch64
  * A pipeline that will reserve and provision aarch64 from Beaker and add them to Jenkins as executors will need to be
    implemented. The same pipeline will execute the existing test coverage for bare metal scenarios on those executors,
    namely:
    * start-stop TS with RHBQ code starters (`startstop-ts-code-quarkus`)
    * start-stop special characters TS (`startstop-ts-special-chars`)
    * bare metal test suite
    * Quarkus Quickstarts
  * We will need to the upstream CI to include aarch64 as a regular platform for their CI.
    * If this is not implemented, we are in risk of catching bugs way too late in release (with our engineering and 
      candidate releases being produced only after upstream release, we would have no way of knowing that upstream stays
      aarch64 certified).

Note: Once we have the jobs for product testing for OCP on aarch64 in both JVM and native mode, we will have three jobs
(in JVM mode, for JDK 17 and 21, and in native mode, for whichever version Mandrel supports), and each of them will be 
installing their test cluster. From previous experience, we can tell that the cluster can handle parallel test suite 
execution. We should consider moving the jobs to the same pipeline, managing its test and test services cluster.

### Impact on resources
Product testing:
* Catch-up on the test coverage on OCP to be same for aarch64 as it is for x86-64
  * One additional OCP cluster for test services in AWS: 6x xlarge x86-64 machines in AWS
  * EUS OCP in AWS: 6x xlarge Graviton machines, 4x large executors in Jenkins for each cell of JDK/OCP version matrix
* Certification of RHBQ native mode on OCP on aarch64
  * medium executor for orchestration
  * 8x xlarge executors added from Beaker, 2 hours execution time
  * ideally, we will be using the same clusters as the other OCP jobs do, but we need at least
    * 1x test cluster - 6x xlarge Graviton machines in AWS
    * 1x test service cluster - 6x xlarge x86-64 machines in AWS
* Certification of both the JVM and native modes in bare metal scenarios on aarch64
  * `startstop-ts-code-quarkus` - 1x large aarch64 machine from Beaker per tested JDK, minutes to tens of minutes 
    execution time
  * `startstop-ts-special-chars` - 1x large aarch64 machine from Beakerper tested JDK, minutes to tens of minutes 
    execution time
  * bare metal test suite
    * JVM mode - 8x large aarch64 machine from Beaker per tested JDK, 1-2 hours execution time
    * native mode - 8x xlarge aarch64 machine from Beaker, 2 hours execution time
  * Quarkus Quickstarts
    * JVM mode - 1x large aarch64 machine fom Beaker per tested JDK, 1 hour execution time
    * native mode - 1x xlarge aarch64 machine form Beaker, 2 hours execution time
* Estimation of increase in time requirements
  * For machine time, parallelism in execution should be established. The spin up of beaker machines does not take that
    long in trials, but we don't have any significantly large sample size yet to say how it will look in long term.
  * For people/investigation time, this depends on how different JDK and test services act on aarch64. So far, 
    significant functional bugs were not present on aarch64. All the issues historically were related to either missing
    test services or missing productized dependencies. For native applications, we have no baseline to base our
    estimates upon.

## Other considerations
* We need to consider how to handle the sprawling matrix. Currently, there are following axes in the matrix:
  * mode (JVM, native)
  * JDK (JDK17, JDK21)
  * OCP version (EUS, stable)
  * container engine (Docker, Podman)

## References
* Feature epic: [QUARKUS-3007 Aarch64/ARM Support for both native and JVM Quarkus apps](https://issues.redhat.com/browse/QUARKUS-3007) 
* QE decomposition: [RHBQ support on aarch64](https://issues.redhat.com/browse/QQE-428)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>