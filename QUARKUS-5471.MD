# QUARKUS-5471 - Add support for MariaDB 11.4

JIRA: https://issues.redhat.com/browse/QUARKUS-5471

Quarkus platform participant Keycloak requires support for MariaDB 11.4 for their Red Hat Build of Keycloak 26.2, which is going to be based on RHBQ 3.20.
For RHBQ 3.15 we supported MariaDB 10.11., 10.5 and 10.3, however MariaDB upstream, only MariaDB 10.11 is [currently maintained](https://mariadb.org/about/#maintenance-policy), while MariaDB 10.3 and 10.5 reached the end-of-life.
Container images provided by Red Hat currently offers MariaDB 10.3, 10.5, 10.11 and only these images should be tested in OpenShift environment.

## Scope of the testing
Change MariaDB image used for bare-metal testing from `docker.io/library/mariadb:10.11` to `docker.io/library/mariadb:11.4`.
The latest supported image used for OpenShift should be switched from `registry.redhat.io/rhel9/mariadb-105:latest` to `registry.redhat.io/rhel9/mariadb-1011:latest`.
The EUS image should stay on `registry.redhat.io/rhel8/mariadb-103:latest` as that's the eldest image version Red Hat offers.

## Existing test coverage
We use the `docker.io/library/mariadb:10.11` image for following bare-metal testing:

- Security JPA
- Multiple persistence units
- SQL application
- DEV mode experience
- Spring Web / Spring Web Reactive
- Panache
- Narayana Transactions
- Hibernate ORM compatibility

We use the `registry.redhat.io/rhel8/mariadb-103:latest` image for following OpenShift testing:

- SQL application
- Multiple persistence units
- Spring Web Reactive

We use the `registry.redhat.io/rhel9/mariadb-105:latest` image for following OpenShift testing:

- Panache
- SQL application
- Spring Web
- Narayana Transactions

### Impact on test suites and testing automation
Images will be changed, no tests will be added or removed.

### Impact on resources
No impact on resources as test coverage didn't change and MariaDB database startup time difference between versions is negligible.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>
