# QUARKUS-2746 - ConsumeEvent annotation: Duplicated event consumption

JIRA link: https://issues.redhat.com/browse/QUARKUS-2746

ConsumeEvent annotation should be invoked once when generic parent class
has an abstract method for it.

Documentation: https://quarkus.io/guides/reactive-event-bus#consuming-events

## Scope of the testing
- There is no upstream coverage.
- A method annotated with the annotation `@ConsumeEvent` must be invoked only once per message.

### Impact on test suites and testing automation
- Quarkus Test Suite

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Read documentation related to `@ConsumeEvent` annotation
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

### Impact on resources
 
 Coverage is going to be focus on `quarkus-vertx` extension. The impact should be minimum in case can be included 
 in one of the existing scenarios.

## Contacts

* Tester: Pablo Gonzalez Granados <pagonzal@redhat.com>