# QUARKUS-8369 - Introduce ability to get response metadata in streamed response

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8369

Upstream PR: https://github.com/quarkusio/quarkus/pull/54533

This PR adds support for accessing HTTP response metadata, such as status code and response headers, while consuming a streamed REST client response through `RestMultiResponse`.

## Scope of the testing

* Consume an SSE stream containing JSON objects:
    * verify all streamed objects are received and the HTTP status and headers are accessible.
* Consume an SSE stream without a `@Produce` annotation:
    * verify the streamed objects and response metadata are still received correctly.
* Consume a stream containing simple String values:
    * verify all streamed values and response metadata are received correctly.
* Consume a streamed response containing `Custom-Header: hello`:
    * verify the client reads the header value `hello` and receives all streamed items.

## Existing test coverage

Upstream tests cover SSE and String streams, access to response status and headers, and operation without a `@Produces` annotation. 

### Impact on test suites and testing automation

* The new tests will be placed in `/http/rest-client-reactive` in the QE test suite.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should only be a few minutes.
* Tests will be executed on baremetal and OpenShift in JVM and native mode. 

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documententation on rest client.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/rest-client#accessing-response-metadata-from-a-stream