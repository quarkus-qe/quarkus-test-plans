# QUARKUS-6233 - Allow mongo client to be configured by a tls registry

JIRA: https://issues.redhat.com/browse/QUARKUS-6233

Allow MongoDB client to be configured by a TLS registry

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/46293

Documentation:
- https://quarkus.io/guides/mongodb#tls-configuration

## Scope of testing
This feature adds a TLS registry configuration for the MongoDB client.
The tests will verify that the MongoDB client, configured via the TLS registry, functions correctly using mutual TLS (mTLS).
Testing of standard TLS is not possible due to MongoDB needs certificates either from trusted certificate authorities, or it's need to specify the `CAFile` for self-signed certificates.
If `CAFile` is specified the mTLS connection is needed.
As tests will use self-signed certificates and not certificates from trusted certificate authorities, the use of mTLS is needed.
The testing of mTLS also covers standard one-way TLS, as creating a successful mTLS connection includes all the necessary steps for a one-way TLS connection.
For more details about setting MongoDB with self-signed certificates, see the links in the References section.

## Automated test development
The tests will be added to testsuite in the `no-sql/mongodb` module.
The new tests will be based on the existing `MongoClientIT`, with modifications to the MongoDB setup.
The MongoDB will be configured to use mTLS connection.
Additionally, negative test cases will be included.
This test cases will be setting the incorrect `tls-configuration-name`, keystore and truststore to verify that TLS registry is used correctly.

### Impact on TS and resources
The tests will be executed on both baremetal and OpenShift environments, covering JVM and native modes.
Expected time increase should be in number of minutes, when the Quarkus app needs to be re-build.
Otherwise, there should be a standard increase in time.

## References
- https://quarkus.io/guides/mongodb#tls-configuration
- https://www.mongodb.com/docs/manual/tutorial/develop-mongodb-locally-with-tls/#std-label-develop-mongodb-locally-with-tls
- https://jira.mongodb.org/browse/SERVER-72839

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
