# QUARKUS-6241 - Allow Hibernate ORM and Hibernate Reactive to be used in the same application

JIRA: https://issues.redhat.com/browse/QUARKUS-6241

Upstream issue: https://github.com/quarkusio/quarkus/issues/13425

Upstream PR: https://github.com/quarkusio/quarkus/pull/44473

Before Quarkus 3.22, it wasn't possible to use the Quarkus Hibernate ORM extension with the Quarkus Hibernate Reactive extension in one project.
Now, users can leverage the combination of Hibernate Reactive and Hibernate ORM.
Support for respective Panache versions will come with Panache 2.
The added documentation mentioned that the Quarkus Hibernate ORM extension can be used for endpoints that synchronously return a value, and the Quarkus Hibernate Reactive extension for reactive endpoints.
Another way to leverage this feature is to use the Quarkus Hibernate Reactive extension when the authentication process needs to access the persistence layer (like a JPA identity provider or an augmentor that loads the `SecurityIdentity` roles from the database), because authentication and authorization are run on the IO thread as well.

## Scope of the testing

Verify that both extensions can be used together in one project.
Use Quarkus Security JPA Reactive as an identity provider and Hibernate ORM for CRUD operations.
Resources need to be secured so that we can verify that authentication and authorization are working properly.

## Existing test coverage

Upstream test coverage includes unit tests for default persistence units, named persistence units, and their combination.
There are also integration tests that are executed in the native mode.

### Impact on test suites and testing automation

A new `security/jpa-reactive` module will be added.
There will be bare-metal tests for JVM and native mode, and an OpenShift test. Two test classes will be added, both tests based on a PostgreSQL database.

### Impact on resources

Test execution time will increase roughly by:

- 1 minute in JVM Mode
- 4 minutes in native mode
- 4 minutes in OpenShift JVM mode
- 8 minutes in OpenShift native mode

## Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [QUARKUS-6261 - Upgrade to Hibernate ORM 7, Hibernate Reactive 3 and Hibernate Search 8](QUARKUS-6261.MD)
