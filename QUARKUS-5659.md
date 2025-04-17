# QUARKUS-5659 Use Arc features in datasource extensions for eager startup and active/inactive

JIRA: https://issues.redhat.com/browse/QUARKUS-5659

Use Arc features in datasource extensions for eager startup and active/inactive

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/41929

Upstream issue:
- https://github.com/quarkusio/quarkus/issues/31328

Documentation:
- https://quarkus.io/guides/hibernate-orm#persistence-unit-active
- https://quarkus.io/version/3.20/guides/datasource#datasource-active

## Scope of testing
The scope will be verify functionality of activating and deactivating datasources.
Activated datasources will be checked to ensure they can perform standard operations.
Deactivated datasources will be checked to confirm they are not enabled.

Additionally, the custom producer for datasources will be tested, leveraging the `InjectableInstance`.
The consumer for this producer will primarily check if the correct datasource was used.
More complex testing of custom producers and consumers won't be part of the scope, as this functionality is primarily intended for extension developers.


## Automated test development
These tests will verify that the `quarkus.datasource."datasource-name".active` property enables and disables selected databases.
The test coverage will be added to the `sql-db/multiple-pus` module, where multiple databases and datasource names are already utilized.
The existing setup of two different databases with three distinct `datasource-name` will be used.

The tests will cover the following scenarios:

- All datasources inactive.
- One datasource active and two inactive.
- One inactive and two active.
- Each active datasource will be checked for its ability to create, obtain, and delete records.
- Each inactive datasource will be checked for the expected error message.

A custom producer will produce a bean that checks for exactly one active datasource, leveraging the `InjectableInstance`.
Two different datasources connected to different databases will be used to verify that the bean is produced correctly.

The test cases will be:
- No active datasource - error will be thrown
- One datasource will be activated - correct database name will be checked

For all defined combinations of active and inactive datasources, the metrics will be checked to ensure that only active datasources are logged.


### Impact on TS and resources
These tests will be added to the `sql-db/multiple-pus` module and executed in both JVM and native modes on bare metal and OCP.
The expected increase in execution time for bare metal is approximately 1 minute for JVM mode native mode.
On OCP, the execution time is expected to increase by around 5 minutes for JVM mode and 10 minutes for native mode.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
