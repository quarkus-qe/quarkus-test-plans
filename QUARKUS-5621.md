# QUARKUS-5621 Quarkus REST - Support record parameter containers

JIRA: https://issues.redhat.com/browse/QUARKUS-5621

Quarkus REST - Support record parameter containers

Upstream PRs:
 - https://github.com/quarkusio/quarkus/pull/42642

Upstream documentation:
 - https://quarkus.io/guides/rest#grouping-parameters-in-a-custom-class

## Scope of the testing
QE team will:
- Get familiar with the functionality
- Assess extensions for further test development
- Explore possible enhancements and propose them upstream if applicable

## Getting familiar with the feature
Getting familiar with
- https://quarkus.io/guides/rest#grouping-parameters-in-a-custom-class
- Java Records support in Quarkus REST
- Explore Java Records support in Spring 

## Test coverage
QE TS will be enhanced to have tests for request parameters mappings into Java Records.
QE TS will be enhanced to add Quarkus REST tests with Java Records combining extensions for OpenAPI and beans validation. 

### Impact on test suites and testing automation
The existing http module(s) will be enhanced with additional scenarios.

### Impact on resources
The expected execution time will increase in the range of seconds for JVM mode.
Native mode can be affected in a range of minutes if a new native binary will be built, otherwise the range of seconds.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## References
- https://quarkus.io/guides/rest#grouping-parameters-in-a-custom-class
- https://www.youtube.com/watch?v=xzOvDoz7XIg
- https://dev.to/psideris89/java-14-records-in-spring-boot-rest-api-n29
- https://github.com/quarkusio/quarkus/issues/42646
