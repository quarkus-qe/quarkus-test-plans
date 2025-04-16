# QUARKUS-5472 - Add support for PostgreSQL 17

JIRA: https://issues.redhat.com/browse/QUARKUS-5472

Quarkus platform participant Keycloak requires support for PostgreSQL 17 for their Red Hat Build of Keycloak 26.2, which is going to be based on RHBQ 3.20.
For RHBQ 3.15 we supported PostgreSQL 16 and 10, however PostgreSQL has already reached [the end of life upstream](https://www.postgresql.org/support/versioning/).
Container images provided by Red Hat currently offers PostgreSQL 12, 13, 15 and 16 and only these images should be tested in OpenShift environment.

## Scope of the testing
Change PostgreSQL image used for baremetal testing from `docker.io/library/postgres:16` to `docker.io/library/postgres:17`.
The EUS image used for OpenShift should be switched from `registry.redhat.io/rhel8/postgresql-10:latest` to `registry.redhat.io/rhel8/postgresql-12:latest`.
The latest supported image used for OpenShift must be kept on `registry.redhat.io/rhel9/postgresql-16:latest` as that's the latest available image.

## Existing test coverage
We use the `docker.io/library/postgres:16` image for following bare-metal testing:

- Vert.x SQL
- XA transactions and SQL application
- Hibernate ORM compatibility
- DEV mode experience and reactive SQL clients
- Narayana transactions
- Hibernate Reactive
- Multiple persistence units
- Hibernate ORM
- Hibernate Search
- Spring Data

We are most of this JVM and native test coverage in OpenShift as well, for which purpose we use the `registry.redhat.io/rhel9/postgresql-16:latest` image.
Only in OpenShift, we test Hibernate ORM, Hibernate Reactive and Hibernate Search with the `registry.redhat.io/rhel8/postgresql-10:latest` image.

### Impact on test suites and testing automation
Images will be changed, no tests will be added. Tests that have explicit PostgreSQL version in their name are going to be renamed (we will replace `10` with `12`).

### Impact on resources
No impact on resources as test coverage didn't change and PostgreSQL database startup time difference between versions is negligible.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>
