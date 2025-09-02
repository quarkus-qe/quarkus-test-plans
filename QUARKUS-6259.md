# QUARKUS-6245 Enable named data sources for Hibernate Reactive

JIRA:  https://issues.redhat.com/browse/QUARKUS-6259

Upstream PRs:
    - https://github.com/quarkusio/quarkus/pull/47631
    - https://github.com/quarkusio/quarkus/pull/49287
    - https://github.com/quarkusio/quarkus/pull/48007

Upstream issues:
    - https://github.com/quarkusio/quarkus/issues/46727

Documentation:
    - https://quarkus.io/version/main/guides/hibernate-reactive#multiple-persistence-units

This feature allow to configure multiple (named) datasources and persistence units with hibernate-reactive.

## Scope of testing
Tests will cover basic working with an app, that is connected to multiple databases.
There will be 3 databases used in total - two of same type (e.g. PostgreSQL) and one different (e.g. MariaDB). 
Goal is to verify if Quarkus with hibernate-reactive correctly handles multiple types of DB at once and if same driver can work with multiple instances of DB simultaneously.
As part of this testing, every DB will be initiated with separate sql-load-script to verify this works properly.

## Existing test coverage
Upstream tests were added as part of https://github.com/quarkusio/quarkus/pull/47631, but these cover only work with one named datasource.
Which is not sufficient, as we can have multiple datasources.
Our testsuite already has a significant coverage for Hibernate Reactive in module `sql-db/hibernate-reactive`.

## Automated test development
Two new test classes will be added into module `sql-db/hibernate-reactive`.
One for bare metal and one for OCP.

## Impact on TS and resources
New test classes will be created, running on JVM and native on both bare metal and OCP.
On bare metal JVM run will take about 2 minute and native 5 minutes. 
On OCP JVM will take about 5 minutes and native 10 minutes.

## Contacts
* Tester: Martin Očenáš <mocenas@redhat.com>
