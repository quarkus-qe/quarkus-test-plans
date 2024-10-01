# QUARKUS-3007: Aarch64/ARM Support for both native and JVM Quarkus apps

Jira: https://issues.redhat.com/browse/QUARKUS-3007

The goal of this feature is to provide production support in the same scope for aarch64 architecture that RHBQ provides
for x86-64 architecture.

For the purposes of planning, the following tasks therefore need to be executed (ordered by priority):
* Same test coverage and certification matrix on OCP on aarch64 for both JVM and native mode as on OCP on x86-64 (see
  [QUARKUS-3446](QUARKUS-3446.md))
* Certification of RHBQ native mode on OCP on aarch64
* Certification of RHBQ in both JVM and native mode in the bare metal scenarios on aarch64 
  (see [QUARKUS-4942](QUARKUS-4942.md))

## Scope of the testing

### General
* Test coverage on OCP should be same for aarch64 as it is for x86-64
  * Tests disabled in `aarch64` profile due to missing test services on that architecture should be re-enabled
* Certification of RHBQ native mode on OCP on aarch64
  * The full native OpenShift coverage will have to be ran in native mode with the Mandrel native builder container
* Certification of both the JVM and native modes in bare metal scenarios will require all the aspects to be tested
  * we support two JDK versions for JVM mode and one JDK version for native mode per release stream
  * bare metal test suite and Quarkus Quickstarts are priority
  * `startstop-ts-code-quarkus`, `startstop-ts-special-chars` are nice to have

### Impact on test suites and testing automation
* Catch-up on the test coverage on OCP to be same for aarch64 as it is for x86-64
  * This means that the aarch64 OCP testing pipeline would require an additional x86-64 cluster to be deployed in AWS
    and test services be prepared before the test execution by the test pipeline. The other part of this is that the
    tests that currently use test services not supported on aarch64 would be moved to standalone test suite module, only
    executed in `aarch64` profile
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
* We will need to run the aarch64 coverage in our periodic builds as running all the coverage planned in previous 
  bullets in candidate releases the first time is too late to catch bugs in the schedule.

Note: Once we have the jobs for product testing for OCP on aarch64 in both JVM and native mode, we will have three jobs
(in JVM mode, for JDK 17 and 21, and in native mode, for whichever version Mandrel supports), and each of them will be 
installing their test cluster. From previous experience, we can tell that the cluster can handle parallel test suite 
execution. We should consider moving the jobs to the same pipeline, managing its test and test services cluster.

### Impact on resources
Product testing:
* Catch-up on the test coverage on OCP to be same for aarch64 as it is for x86-64
  * One OCP cluster for test services in AWS: 6x xlarge x86-64 machines in AWS
* Certification of RHBQ native mode on OCP on aarch64
  * medium executor for orchestration
  * 9x xlarge executors added from Beaker, 2 hours execution time
  * ideally, we will be using the same clusters as the other OCP jobs do, but we need at least
    * 1x test cluster - 6x xlarge Graviton machines in AWS
    * 1x test service cluster - 6x xlarge x86-64 machines in AWS
* Certification of both the JVM and native modes in bare metal scenarios on aarch64
  * bare metal test suite
    * JVM mode - 9x large aarch64 machine from Beaker per tested JDK, 1-2 hours execution time
    * native mode - 9x xlarge aarch64 machine from Beaker, 2 hours execution time
  * Quarkus Quickstarts
    * JVM mode - 1x large aarch64 machine fom Beaker per tested JDK, 1 hour execution time
    * native mode - 1x xlarge aarch64 machine form Beaker, 5 hours execution time
* Estimation of increase in time requirements
  * Beaker and OCP provisioning run in parallel and complete within 2 hours.
  * Test jobs execute in parallel with x86-64 coverage on different machine quota.
  * Running in parallel with x86-64 coverage, the pipeline should finish within 10 hours.

## Other considerations
* We should enable our other functional and structural coverage, but quickstarts and bare metal test suite are priority. 
  * `startstop-ts-code-quarkus`, `startstop-ts-special-chars`
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