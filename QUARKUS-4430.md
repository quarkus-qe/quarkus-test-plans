# QUARKUS-3535 - Add a metric for rejected submissions to the pool in RHBQ 3.2

JIRA: https://issues.redhat.com/browse/QUARKUS-4430

PR: https://github.com/quarkusio/quarkus/pull/39583

Upstream issue: https://github.com/quarkusio/quarkus/issues/39581

Verify that URIs in metrics tags are correctly recorded when they match a client or server pattern,
especially in cases of redirection (302) and not found (404) responses.

## Scope of the testing
* Run Quarkus micrometer Prometheus application and verify metric tags for:
   * Redirections (HTTP 302)
   * Not Found responses (HTTP 404)
   * Empty path responses (HTTP 204)
   * Dynamic path segments (HTTP 301)

* The test coverage will be verified for JVM and Native.

## Existing test coverage
* We have already tests related to monitoring metrics with Prometheus but not specific test
  about this improvement feature related to tag redirections and not found responses recorded in metrics.
 

### Impact on test suites and testing automation
* New test class will be added into [monitoring](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/monitoring/micrometer-prometheus) 

### Impact on resources
* One baremetal test execution in JVM mode and native mode.
* Overall QE Test Suite execution time will be greater roughly by a few seconds

## Contacts
* Tester: Jose Carranza <jcarranz@redhat.com>

## References
- [http metrics should provide a path instead of REDIRECTION and NOT_FOUND](https://github.com/quarkusio/quarkus/issues/39581)