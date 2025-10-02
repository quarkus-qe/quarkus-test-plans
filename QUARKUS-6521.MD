# QUARKUS-6521 - Virtual thread support for smallrye-graphql

JIRA: https://issues.redhat.com/browse/QUARKUS-6521

Upstream PR: https://github.com/quarkusio/quarkus/pull/47802

Quarkus 3.25 adds support for running GraphQL endpoints on virtual threads.
Previously, endpoint callbacks could be executed either on an IO thread or a worker thread.
This new feature was a minor update to the GraphQL extension.
When the GraphQL extension executes blocking code, it checks whether the endpoint is annotated with the RunOnVirtualThread annotation and selects the virtual thread executor instead of the worker thread executor.

## Scope of the testing

Automated test coverage should verify that:

* All Quarkus GraphQL endpoints with a synchronous return type or those annotated @Blocking can be successfully migrated to and executed on virtual threads.
* GraphQL endpoint code does not pin the carrier thread.
* Security, telemetry, and GraphQL clients integration works with this new feature.
* PostgreSQL JDBC driver can be used to access the database from virtual threads. Quarkus documentation warns that most JDBC drivers still pin the carrier thread. With the [JEP 491](https://openjdk.org/jeps/491) and Java 25, the situation is likely to improve. Until then, our goal is to verify that users can at least access the database from virtual threads.
* OpenShift scenarios, JVM mode, DEV mode, and native mode scenarios.

Manual verification should assert low memory consumption and application performance under heavy load.

## Existing test coverage

There is no virtual threads test coverage in any of our Quarkus QE test suites.
The one test that uses the RunOnVirtualThread annotation is a copy-and-paste of the original reproducer, and the original issue wasn’t caused by using virtual threads.

Upstream test coverage verifies that GraphQL endpoints annotated with the RunOnVirtualThread annotation are executed on virtual threads, and the carrier thread is not pinned.
The detection of pinning cases relies on JFR support.
The synchronization block, relevant for Java 21, 22, and 23, is also tested.

### Impact on test suites and testing automation

* GraphQL blocking test cases in the http/graphql module will be mirrored by the virtual thread test cases, all tested by existing test classes. A new test class will be required for a DEV mode scenario, which is currently not tested.
* The http/graphql-client module with a new test class that will verify a combination of GraphQL endpoints executed on virtual threads with the GraphQL client.
* Virtual thread metrics and pinning cases for GraphQL endpoints will be verified with Micrometer binder. This will require a new test class in the http/graphql-telemetry.
* The security/jwt module already has GraphQL scenarios, and SmallRye JWT authentication requires blocking; therefore, we will enhance existing test coverage with virtual thread scenarios and verify integration of the new feature with Quarkus Security.
* Combination with JDBC PostgreSQL will be tested in the sql-db/multiple-pus module, where we can extend the existing test class with the GraphQL test cases. This test module already contains the Micrometer Prometheus test coverage; therefore, we can use the virtual thread test binder to monitor the pinning.
* Tests will run with Java 21 and higher.

### Impact on resources

The addition of the three new test classes for bare-metal tests and the two new test classes for their execution on OpenShift will increase execution time by roughly 10 minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Quarkus Virtual Thread Support Reference](https://quarkus.io/guides/virtual-threads)
