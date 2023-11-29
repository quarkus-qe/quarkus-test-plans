# QUARKUS-3535 - Add a metric for rejected submissions to the pool in RHBQ 3.2

JIRA: https://issues.redhat.com/browse/QUARKUS-3535

PR: https://github.com/quarkusio/quarkus/pull/35541

Upstream issue: https://github.com/quarkusio/quarkus/issues/35540

One simple way to deal with load shedding is to limit thread pool queue size, however
that also brings downside of limited number of requests that can be handled of time.
Applications need to measure rejected pool submissions metric so that load can be spread.

## Scope of the testing
 * Run Quarkus application with limited thread pool queue and maximal number of thread
 * Assert thread pool configuration properties have affect and under big load pool submissions are rejected
 * Run test on baremetal only as feature is related to JVM wherever it is run
 * Run test in the JVM mode only as rejection is next to impossible reproduce in native mode

## Existing test coverage
* We already test in the QE Test Suite HTTP Vert.x module ability to retrieve metrics in OpenShift and native mode.

### Impact on test suites and testing automation
 * New test will verify metric for rejected submissions to the Vert.x worker thread pool

### Impact on resources
 * One baremetal test execution in JVM mode
 * Test will not require any containers and will be executed very fast
 * Overall QE Test Suite execution time will be greater roughly by 10 seconds

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [ExecutorService should expose metrics](https://github.com/quarkusio/quarkus/issues/34998)