# QUARKUS-7864 - GraphQL client TLS reload

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7864

Upstream PR: https://github.com/quarkusio/quarkus/pull/53856

This feature allows **application-scoped** GraphQL clients to react to TLS configuration reloads in-place, without requiring the client to be recreated. Previously, TLS reload only took effect if the client had a shorter CDI scope, forcing recreation on each reload.

## Scope of the testing

* Verify TLS configuration reload for an already created **application-scoped typesafe** GraphQL client:
    * Start the client with a valid but untrusted client certificate.
    * Verify the mTLS GraphQL request fails.
    * Replace the configured certificate with a trusted certificate.
    * Reload the TLS configuration and fire `CertificateUpdatedEvent`.
    * Verify that the same running GraphQL client successfully performs the next request.

* Verify the same as above for an already created **application-scoped dynamic** GraphQL client.

## Existing test coverage

Upstream tests cover TLS certificate reload for application-scoped typesafe and dynamic GraphQL clients. They verify that when a certificate is updated and a `CertificateUpdatedEvent` is fired, the already-running GraphQL client picks up the new certificate without needing to be restarted or recreated.

QE tests will expand on upstream coverage by verifying TLS reload from a user's perspective, using real certificates, mTLS and an existing GraphQL client.

### Impact on test suites and testing automation

* The new tests will be placed in `http/graph-client`.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should only be a few minutes.
* Tests will be executed on baremetal and OpenShift in JVM and native mode. 

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documentation on tls registry.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/tls-registry-reference#reloading-certificates