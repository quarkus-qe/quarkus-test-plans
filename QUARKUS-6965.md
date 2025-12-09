# QUARKUS-6965 Keep the Hibernate ORM and Reactive extensions enabled even when no entity is defined

JIRA: https://issues.redhat.com/browse/QUARKUS-6965

Upstream issue: https://github.com/quarkusio/quarkus/issues/7148

Upstream PRs:
* https://github.com/quarkusio/quarkus/pull/48732 - main PR for the new feature
* https://github.com/quarkusio/quarkus/pull/48783
* https://github.com/quarkusio/quarkus/pull/50262

New feature allow to start Hibernate ORM/reactive even if no entity is defined to use it.
It is intended for users that want to use Hibernate for direct usage of database through Hibernate,
for cases like executing native queries using `StatelessSession`.
It still requires a PU, and it's datasource to be active, otherwise Hibernate will not start.

New feature is primarily implemented in the PR [48732](https://github.com/quarkusio/quarkus/pull/48732).
Other PRs [48783](https://github.com/quarkusio/quarkus/pull/48783) and [50262](https://github.com/quarkusio/quarkus/pull/50262) are mostly about behaviour tweaks and fixes. 

## Scope of testing
Verify the feature to have Hibernate ORM/reactive started without any entity defined.
This will require creating test app, which will use no Hibernate entities, but will allow native SQL queries.
It will also verify usage of Hibernate repository without any entity.

Starting Hibernate can be disabled at build time by setting `quarkus.hibernate-orm.enabled=false`.
Also, it will also be disabled at runtime if no datasource is active.

Goal is to verify working scenario with entity-less app as well as scenarios where Hibernate is not started.

## Existing test coverage
Quarkus QE TS has test coverage for Hibernate ORM and Hibernate reactive, but not for this case.

## Impact on test suite
A new test classes will be added into modules `hibernate/hibernate-orm` and `hibernate/hibernate-reactive`.
There will be a test classes for bare-metal and OCP. 

## Impact on resources
Multiple new test classes will be added, and executed against JVM and native on both baremetal and OCP.
Possibly multiple app builds will be required to test all scenarios.
Rough expectation is to add about 5 minutes to JVM and 12 minutes native on baremetal.
Slightly bigger number of minutes on OCP.

## Contacts
* Tester: Martin Ocenas <mocenas@ibm.com>