# QUARKUS-6244 - Add Jakarta Data documentation, dependency management and tests

JIRA: https://issues.redhat.com/browse/QUARKUS-6244

Upstream issues: https://github.com/quarkusio/quarkus/issues/44278, https://github.com/quarkusio/quarkus/issues/35603

Upstream PR: https://github.com/quarkusio/quarkus/pull/47012, https://github.com/quarkusio/quarkus-quickstarts/pull/1516

Starting with Quarkus 3.22, Quarkus documents support for Jakarta Data 1.0.1 when used with Hibernate ORM extension.
This feature relies heavily on the `hibernate-jpamodelgen` annotation processor and is implemented almost entirely outside the Quarkus.
Quarkus itself plans to integrate Jakarta Data support through Panache extension and complement the active record pattern.
The QUARKUS-6244 itself is about vanilla Hibernate ORM support for Jakarta Data 1.0.1.
Jakarta Data goals are simplifying data access and their manipulation (e.g. by making sessions stateless), unify API (Jakarta NoSQL and Persistence specs), abstracting layer common for different databases and storage sources and so on.
That means that we need to test all supported databases.
We need to verify integration points between Quarkus and Hibernate ORM as the vast majority of upstream test coverage covers vanilla Hibernate ORM.
There is no clear connection of this feature to OpenShift or any other environment, therefore we just run a smoke test.

## Scope of the testing

Testing must focus on Jakarta Data integration with Quarkus for supported configurations and environments:

- all functionalities shall be tested for PostgreSQL, MariaDB, MySQL, Oracle, SQL Server and DB2
- changes to a static metamodel will be tested in a DEV mode
- smoke test for OpenShift will be run with PostgreSQL (as that is one of the most popular databases)
- the functionalities won't be tested comprehensively, we need smoke tests for each of them, namely:
  - Jakarta Data Annotations (e.g. `@Repository`) including their attributes (e.g. the `@Repository` attribute `provider`)
  - built-In interfaces (e.g. `CrudRepository`)
  - data retrieval and modifications (insert, update, save, delete for the modification and find, count and exists for the retrieval)
  - reusing programming model for entity classes from Jakarta Persistence
  - compatibility with Jakarta Persistence entities and queries using Jakarta Data Query Language
  - flexible sorting, paging and precedence of sort criteria
  - type-safety using generated classes from the static metamodel
  - direct field access for basic fields, embedded fields and association fields
  - trivial entity associations (not required by specs, but most likely to be tried by users)
  - entity type inference (either exactly the same type or `Limit`, `Order`, `PageRequest`, `Sort`)
  - direct access to the connection from a resource accessor method
  - lifecycle annotations
- we also need to ensure good integration with functionalities provided by other extensions:
  - JTA transactions (both on bare-metal and OpenShift)
  - CDI (e.g. possibility to inject the supertypes of actual repository types and intercept repository interface invocations)
  - bean validation for both entities (e.g. ensure validation annotations on entity fields are enforced) and repository method parameters

## Existing test coverage

Tests in Quarkus main project are too simple and coverage is sporadic.
Tests added to Quarkus Quickstarts are hardly better.
True test coverage resides in Hibernate ORM project https://github.com/hibernate/hibernate-orm/tree/main/tooling/metamodel-generator/src/jakartaData/java.
I presume the logic behind this choice is that this feature relies on a static model generation which helps with a type-safety, but also, with native-mode compatibility (limits need for registrations for reflection).
Nevertheless, missing DEV mode tests for the static model generation are big pain as they don't verify user experience in the DEV mode.
There is no test coverage in Quarkus QE test suites.

### Impact on test suites and testing automation

A new module dedicated to Jakarta Data will be added under `sql-db` module.
Each tested database will have its own test class for JVM / native mode.
Extra test classes will be required for the OpenShift test and the DEV mode test.
Functionalities tested in OpenShift JVM mode and bare-metal JVM mode will be same.

### Impact on resources

Test execution time will increase very roughly by 1 h 10 minutes due to:

- 6 databases * 4 minutes for a JVM mode test = 24 minutes
- 6 databases * 8 minutes for a native mode test = 48 minutes
- 1 PostgreSQL smoke test for OpenShift = 5 minutes
- 1 PostgreSQL smoke test for DEV mode = 5 minutes

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Jakarta Data 1.0 Specification Document](https://jakarta.ee/specifications/data/1.0/jakarta-data-1.0.pdf)
- [Jakarta Data and Jakarta Persistence by Gavin King](https://www.youtube.com/watch?v=X9GplCb5SWY)
- [Quarkus Insights #201: Jakarta Data in Quarkus](https://www.youtube.com/watch?v=ijcoMZyKmsA)
- [Getting Started with Jakarta Data and Hibernate](https://thorben-janssen.com/getting-started-with-jakarta-data/)
- [Hibernate Data Repositories](https://hibernate.org/repositories/)
