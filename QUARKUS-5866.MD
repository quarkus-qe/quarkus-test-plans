# QUARKUS-5866 - REST client support of TLS configuration updates

JIRA: https://issues.redhat.com/browse/QUARKUS-5866

Upstream PR: https://github.com/quarkusio/quarkus/pull/46961

Our customer reported that certificate reload of TLS certificates is not working in OpenShift when Quarkus runs as a native executable.
From details, we know or assume, they have native applications running in OpenShift and communicating using a REST clients.
Considering that when CA certificate bundle is mounted as a secret it's value does not change with the secret value change, our customer most likely used projected volume whose value change is reflected.
The customer reported that when the mounted service CA certificate was rotated, the REST client still used the old certificate loaded in a memory.
This is not a bug as we didn't support certificate reloads for the Quarkus REST client, and we still don't support it for Quarkus RESTEasy client, Keycloak Admin client and so on.
While the certificate reload will work for programmatically built REST clients if and only if built with the `io.quarkus.rest.client.reactive.runtime.RestClientBuilderImpl`, this class is only mentioned once in the documentation in a context of a proxy configuration.
Therefore, I do not expect that we need to care about programmatically built clients.
Quarkus implements the clients created via `@RegisterRestClient` using this builder anyway, so no difference.

## Scope of the testing
Test that Quarkus REST client supports periodic certificate reloading for Quarkus applications run in native and JVM mode.
We need to run bare-metal tests with the certificates loaded from a local file system, as well as OpenShift serving certificates scenarios.
Our bare-metal scenarios can play with valid and invalid certificates a bit, to assure that repeated rotation works.
For the OpenShift scenarios, we will manually rotate the generated service certificate and assure that Quarkus picked up the new certificate and use it.
The mounted TLS certificate and key from the serving certificate secret are not updated by default.
Standard way to apply changes in the served certificate is to delete pod, however we want to test certificate reloading so we will modify mounted file manually.
Whole reload process is asynchronous and the certificates in the underlying client used by the REST client wrapper are updated after the certificate reload has been executed.
That means that we need to have waited a bit and can't expect everything to work the moment after the CDI event reported successful reload.

## Existing test coverage
Upstream test coverage is a JVM mode test executed in a one process, the CDI event that triggers the certificate reloading is triggered from inside the JUnit test method (in other words, not a black box test).
The test starts with a wrong scenario and asserts successful recovery, the certificate reload is triggered via dedicated CDI event.

### Impact on test suites and testing automation
Both OpenShift and bare-metal scenarios will be added to the `http/rest-client-reactive` module of our Quarkus QE Test Suite.
We already have there OpenShift certificate serving scenarios and new additions will be integrated into existing test classes, `OpenShiftServingCertificatesIT` and `ReactiveRestClientIT`, thus limiting extra time required for a new native build.

### Impact on resources
Considering the test coverage will be integrated into existing test classes, I expect increase in the test execution time roughly in 3-4 minutes.
There will be time between certificate rotation and certificate reload so that tests are not flaky, otherwise HTTP requests will take just few seconds.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>
