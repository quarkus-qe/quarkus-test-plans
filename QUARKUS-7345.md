# QUARKUS-7345 Rework graceful shutdown to be more graceful WRT HTTP

JIRA: https://issues.redhat.com/browse/QUARKUS-7345

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/50975

The upstream PR changes handling of HTTP requests during the shutdown of Quarkus app, so it will not return 503 error.

## Existing test coverage
Upstream test coverage includes checks of this behavior on a level of Quarkus unit tests 

## Scope of the testing
Testing will focus on integration tests. 

Scenarios to verify
- There are no regressions (this is covered by the whole test suite)
- Error 503 is not returned for http 1.1
- Error 503 is not returned for http 2
- This behavior can be observed for reactive and ordinary endpoints

OCP is out of scope for this effort, since it has its own shutdown policies

## Impact on test suites and testing automation
There will be new tests in either lifecycle application module or http-minimum[-reactive] modules 

## Impact on resources
The expected execution time will be increased for 1-2 minutes for JVM mode and ~5 minutes for native mode.

## Contacts
* Tester: Fedor Dudinskii <fdudinsk@ibm.com>

## References
- https://github.com/quarkusio/quarkus/issues/50625
