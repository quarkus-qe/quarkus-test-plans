# QUARKUS-715 Support for multiple Hibernate ORM persistence units

Jira: https://issues.redhat.com/browse/QUARKUS-715

GitHub: https://github.com/quarkusio/quarkus/issues/2835

This is an overview document drafting out test plan for support of multiple persistence units within a Quarkus 
application. The persistence units are configured by `application.properties` file.

## Scope of the testing

### Component testing
The feature consists of boilerplate code allowing the Quarkus applications to hold more than one Hibernate persistence 
unit in their context.

Hibernate functionality is tested on [Hibernate CI](https://ci.hibernate.org/).

### Integration testing
A set of integration tests has been implemented and merged with the [feature implementation](https://github.com/quarkusio/quarkus/pull/11322/files).
These tests are executed in Quarkus CI on upstream on a subset of supported platforms.

An integration test verifying correctness of the feature on OpenShift clusters will be implemented in
[quarkus-qe/quarkus-openshift-test-suite](https://github.com/quarkus-qe/quarkus-openshift-test-suite). The test will be
an extension of existing SQL test cases to use application with multiple Hibernate persistence units.

A new module will be added to [quarkus-qe/beefy-scenarios](https://github.com/quarkus-qe/beefy-scenarios/tree/main/004-quarkus-HHH-and-HV)
with an application using multiple persistence units with MariaDB, PostgreSQL and MSSQL.

### Platform testing
Tests are going to be executed against all tested versions of databases, RHEL and OpenShift versions. 
Both the JVM and native mode is going to be tested.

## Impact on resources
- A new module will be added to [quarkus-qe/beefy-scenarios](https://github.com/quarkus-qe/beefy-scenarios/tree/main/004-quarkus-HHH-and-HV).
  Tests will required containerized databases. Timeframe for execution is estimated to tens of minutes for both the JVM
  and native mode.
- Implementation of deployment using multiple persistence units within OpenShift test suite will double the time of 
  execution of `sql-db` modules in [quarkus-qe/quarkus-openshift-test-suite](https://github.com/quarkus-qe/quarkus-openshift-test-suite)
  for all tested platforms.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use
- Check the impact on disk + memory footprint and boot time

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature issue: [QUARKUS-715 Support for multiple Hibernate ORM persistence units](https://issues.redhat.com/browse/QUARKUS-715)
* Upstream feature issue: [Support multiple persistence units #2835](https://github.com/quarkusio/quarkus/issues/2835)
* Upstream documentation: [Quarkus - Using Hibernate ORM and JPA # Multiple persistence units](https://quarkus.io/guides/hibernate-orm#multiple-persistence-units)
