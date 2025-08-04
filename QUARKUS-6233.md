# QUARKUS-6233 - Allow mongo client to be configured by a tls registry

JIRA: https://issues.redhat.com/browse/QUARKUS-6233

Allow mongo client to be configured by a tls registry

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/46293

Documentation:
- https://quarkus.io/guides/mongodb#tls-configuration

## Scope of testing
This feature adding TLS registry configuration for mongodb client.
The test will cover that mongodb client with TLS registry working correctly using mTLS.
The testing of TLS is not possible due to mongodb needs certificates from trusted certificate authorities, or it's need to specify the `CAFile` for self-sing certificates.
If `CAFile` is specified the mTLS connection is needed.

## Automated test development
The test will be added to testsuite in module `no-sql/mongodb`.
The `MongoClientIT` tests will be same only the setup of mongodb will be different.
The coverage will be for baremetal and Openshift.


### Impact on TS and resources
Expected time increase should be in number of minutes, when the Quarkus app need to be re-build.
Otherwise, there should be standard increase of time.

## References
- https://quarkus.io/guides/mongodb#tls-configuration
- https://www.mongodb.com/docs/manual/tutorial/develop-mongodb-locally-with-tls/#std-label-develop-mongodb-locally-with-tls
- https://jira.mongodb.org/browse/SERVER-72839

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
