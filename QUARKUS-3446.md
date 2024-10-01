# QUARKUS-3446: Enabling disabled OCP coverage on aarch64

Jira: https://issues.redhat.com/browse/QUARKUS-3446

This issue is about enabling tests for OpenShift that have been disabled on aarch64 due to missing test services with
RHBQ 3.8 release, as a followup to missed targets on OCP planned in [QUARKUS-3007](QUARKUS-3007) with RHBQ 3.8.

## Scope of the testing

### General
* OpenShift Serverless coverage
* Red Hat AMQ Streams coverage
* MongoDB coverage
* Red Hat SSO coverage
  * Note that this coverage can only be re-enabled after RHBQ 3.15 release, as RH SSO is layered on RHBQ 3.15.

### Impact on test suites and testing automation
* Re-enabling tests disabled with the following issues as reason:
  * https://github.com/quarkus-qe/quarkus-test-suite/issues/1142
  * https://github.com/quarkus-qe/quarkus-test-suite/issues/1144
  * https://github.com/quarkus-qe/quarkus-test-suite/issues/1146
  * https://github.com/quarkus-qe/quarkus-test-suite/issues/1147
  * https://github.com/quarkus-qe/quarkus-test-suite/issues/1145 (after 3.15)

### Impact on resources
Product testing:
* 0 impact on resources. Will run on the same infrastructure as existing OCP coverage on aarch64. 
* Increase in time in order of tens of minutes, as the tests are spread into matrix and running in parallel.

## References
* Feature story: [QUARKUS-3446 Enabling disabled OCP coverage on aarch64](https://issues.redhat.com/browse/QUARKUS-3446)
* QE decomposition:[Ensure same coverage for JVM on OCP on aarch64 as on x86-64](https://issues.redhat.com/browse/QQE-258)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>