# QUARKUS-2745 - OutboundSseEvent serialization

JIRA link: https://issues.redhat.com/browse/QUARKUS-2745

OutboundSseEvents are not correctly serialized.

Documentation: https://quarkus.io/guides/resteasy-reactive#server-sent-event-sse-support

## Scope of the testing
- Verify that a SSE endpoint that returns a raw array of `OutboundSseEvent` objects is not throwing any error and the generated object is the expected one.
- Must work on JVM and Native mode

### Impact on test suites and testing automation
- Quarkus Test Suite

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Read documentation related to SSE
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

### Impact on resources
 
 Coverage is going to be focus on `quarkus-resteasy-reactive` extension. The impact should be minimum because is going to be part of one of the existing scenarios. 

## Contacts

* Tester: Pablo Gonzalez Granados <pagonzal@redhat.com>