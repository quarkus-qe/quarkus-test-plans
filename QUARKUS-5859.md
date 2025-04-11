# QUARKUS-5859 Support permission checkers for WebSockets Next

JIRA: https://issues.redhat.com/browse/QUARKUS-5859

Support permission checkers for WebSockets Next

Upstream PRs:
 - https://github.com/quarkusio/quarkus/pull/45704

Upstream documentation:
 - https://quarkus.io/guides/websockets-next-reference#secure-endpoints-with-permission-checkers

## Scope of the testing
QE team will:
- Get familiar with the functionality
- Assess test coverage provided as part of the PR
- Explore possible enhancements of the test coverage and propose them upstream or into QE TS

## Getting familiar with the feature
Getting familiar with
- https://quarkus.io/guides/websockets-next-reference#secure-endpoints-with-permission-checkers
- https://quarkus.io/guides/security-authorize-web-endpoints-reference#permission-checker
- Discussing with developer topics connected to:
  - Checker method parameters constraints
  - Return values of PermissionChecker
  - Documentation concerns, e.g. no fallback/symptoms documented for missing checkers
  - Uni or Multi are not explicitly covered in the text and test
  - Fail strategy - what if no error handler / `@OnError` is defined

## Test coverage
Upstream test coverage will be enhanced by the topics for the discussion with the developer.
QE TS will be enhanced to have permission checkers tests and to cover them also in native mode.

### Impact on test suites and testing automation
The existing `websockets/websocket-next` module will be enhanced with additional scenario.

### Impact on resources
The expected execution time will increase in the range of seconds for JVM mode. 
The same applies to native mode, build of new native binary is not anticipated.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## References
- https://quarkus.io/guides/websockets-next-reference#secure-endpoints-with-permission-checkers
- https://quarkus.io/guides/security-authorize-web-endpoints-reference#permission-checker
