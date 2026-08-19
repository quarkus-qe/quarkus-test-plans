# QUARKUS-7819 - Add CORS support to the management interface

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7819

Upstream PR: https://github.com/quarkusio/quarkus/pull/53432

This PR adds dedicated CORS configuration for the Quarkus management interface using the quarkus.management.cors.* properties.

Management CORS is disabled by default, to make your Quarkus management interface accessible to another application running on a different domain, CORS must now be explicitly configured. Also when CORS is enabled without configuring specific origins, only same origin requests are allowed.

## Scope of the testing

* Verify that a configured origin can access management endpoints such as /q/health.
* Verify that an origin not included in the management CORS configuration is rejected with HTTP 403.
* Verify that the same origin requests to the management interface are allowed.
* Verify that the application and management interfaces can use different CORS policies.
* Verify that when management CORS is disabled, request to /q/health succeeds but the response does not contain an Access-Control-Allow-Origin header.

## Out of Scope
* Testing every management endpoint, /q/health will be used as the representative.
* Running a real client application or browser, requests are simulated like other REST tests.

## Existing test coverage

Upstream tests cover allowed, rejected and same origin requests to the management interface.

The QE test suite does not currently cover management CORS. The new tests will cover upstream scenarios and also verify that application and management interfaces can be configured with different allowed origins.

### Impact on test suites and testing automation

* The new tests will be placed in the `http/management` module.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should only be a few minutes.
* Tests will be executed on baremetal and OpenShift in JVM and native mode. 

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documentation on the management interface and CORS.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/management-interface-reference