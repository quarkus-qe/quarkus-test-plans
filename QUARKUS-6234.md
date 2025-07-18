# QUARKUS-6234 - Compose Dev Service

JIRA: https://issues.redhat.com/browse/QUARKUS-6234

Compose Dev Services, planned to have dev-support scope.

Upstream PR:
- https://github.com/quarkusio/quarkus/pull/46848

Followup PRs:
- https://github.com/quarkusio/quarkus/pull/48325
- https://github.com/quarkusio/quarkus/pull/48335
- https://github.com/quarkusio/quarkus/pull/48472

Upstream documentation:
- https://quarkus.io/guides/compose-dev-services

## Scope of the testing
QE team will:
- Get familiar with the functionality
- Assess test coverage provided as part of the main PR #46848
- Assess documentation provided as part of the #46848 and #48472 PRs
- Explore possible enhancements of the existing upstream test coverage and documentation
- Add scenarios for database images we use in QE TS

## Getting familiar with the feature
Getting familiar with
- Documentation and related PRs
- Docker Compose support in Spring Boot

## Test coverage
Upstream codebase
- Review of existing devservices to ensure Docker Compose support is in place
  - Discussion with the developer about the identified ones, GitHub issues creation
  - Implementing the support for missing ones is out of the scope of this effort
- Ensure Docker Compose examples provided as part of the #46848 and #48472 PRs are tested
  - Additional upstream coverage will be created if something is missing
- Ensure Docker Compose examples are provided for all the devservices in the codebase
  - Discussion with the developer about the identified ones, GitHub issues creation
  - Implementation of the missing examples is optional
- Ensure upstream test coverage is executed regularly and in both JVM and Native mode
  - Upstream CI adjustment if some parts of the test coverage are not executed
- Documentation review to check that the described configuration customizations are covered in tests
  - Implementation of the coverage for missing ones is optional
  - This feature is not production facing
- Documentation review with a focus on user experience
  - Looking at topics like sufficient introduction to the topics covered in the sections, typo check, correctness of used properties
- Documentation comparison with the Spring Boot documentation for Docker Compose support
  - Discussion with the developer about the identified topics, GitHub issues creation

QE TS codebase
- Will be enhanced with scenarios for database images used in QE TS
- Focus on the correct detection of the containers
    - https://quarkus.io/guides/compose-dev-services#supported-extensions-and-discovery-criteria

### Impact on test suites and testing automation
The existing `sql-db/sql-app` module will be enhanced with additional scenarios.

### Impact on resources
The expected execution time will increase by several minutes for JVM mode.
Native mode and OpenShift scenarios are out of scope, the primary target for this feature is dev mode.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## References
- https://quarkus.io/guides/compose-dev-services
