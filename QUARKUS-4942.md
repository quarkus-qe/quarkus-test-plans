# QUARKUS-4942: Enabling Quarkus test coverage running on baremetal aarch64

Jira: https://issues.redhat.com/browse/QUARKUS-4942

This issue is about running the same coverage we run in bare metal testing for RHBQ on x86-64, as a followup to 
deprioritised targets on bare metal planned in [QUARKUS-3007](QUARKUS-3007) with RHBQ 3.8.

## Scope of the testing

### General
* Certification of both the JVM and native modes in bare metal scenarios will require all the aspects to be tested
    * we support two JDK versions for JVM mode and one JDK version for native mode per release stream
    * bare metal test suite and Quarkus Quickstarts are priority
    * `startstop-ts-code-quarkus`, `startstop-ts-special-chars` are nice to have, but they are not priority for 3.15

### Impact on test suites and testing automation
* Certification of both the JVM and native modes in bare metal scenarios on aarch64
    * product aarch64 testing pipeline should be extended with:
      * provisioning `large` label machines for running JVM mode coverage
      * bare metal test suite, JVM and native mode
      * Quarkus Quickstarts, JVM and native mode
    * `startstop-ts-code-quarkus`, `startstop-ts-special-chars` are nice to have, but they are not priority for 3.15

### Impact on resources
* Certification of both the JVM and native modes in bare metal scenarios on aarch64
    * bare metal test suite
        * JVM mode - 9x large aarch64 machine from Beaker per tested JDK, 1-2 hours execution time
        * native mode - 9x xlarge aarch64 machine from Beaker, 2 hours execution time
    * Quarkus Quickstarts
        * JVM mode - 1x large aarch64 machine fom Beaker per tested JDK, 1 hour execution time
        * native mode - 1x xlarge aarch64 machine form Beaker, 5 hours execution time
          Estimation of increase in time requirements
* Beaker provisioning should complete within 2 hours.
* Test jobs execute in parallel with x86-64 coverage on different machine quota.
* Running in parallel with x86-64 coverage, the bare metal jobs should finish within 10 hours.

## Other considerations
* We should enable our other functional and structural coverage, but quickstarts and bare metal test suite are priority.
    * `startstop-ts-code-quarkus`, `startstop-ts-special-chars`
* We need to consider how to handle the sprawling matrix. Currently, there are following axes in the matrix:
    * mode (JVM, native)
    * JDK (JDK17, JDK21)
    * container engine (Docker, Podman)

## References
* Feature story: [QUARKUS-4942 Enabling Quarkus test coverage running on baremetal aarch64](https://issues.redhat.com/browse/QUARKUS-4942)
* QE decomposition:
    * [Ensure support for RHBQ in native mode on RHEL 8](https://issues.redhat.com/browse/QQE-433)
    * [Ensure support for RHBQ in JVM mode on RHEL 8](https://issues.redhat.com/browse/QQE-437)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>