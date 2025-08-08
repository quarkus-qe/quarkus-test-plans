# QUARKUS-6242 - Apply validation modes to the Hibernate Reactive session factory config / Add tests for Reactive + Validator

JIRA: https://issues.redhat.com/browse/QUARKUS-6242

Upstream PR:
- https://github.com/quarkusio/quarkus/pull/46398

Upstream documentation:
- https://quarkus.io/guides/hibernate-orm#validator_integration

PR #46398 introduces support for validation modes in Hibernate Reactive, aligning it with Hibernate ORM. 
It enables developers to control how and when Bean Validation constraints (e.g., @NotNull, @Size) are applied to reactive entities.

## Scope of the testing
QE team will:
- Get familiar with the functionality
- Assess test coverage and documentation provided as part of the main PR #46398
- Explore possible enhancements of the existing upstream test coverage and documentation
- Extend scenarios in the `sql-db/hibernate-reactive` module

## Getting familiar with the feature
- Review and understand the feature and validation modes:
    - `auto` (default): runtime + DDL
    - `callback`: runtime only
    - `ddl`: schema/DDL only
    - `none`: no validation
- Understand the difference in behavior between the modes:
    - When and where validation is expected
    - Expected errors vs. schema-level failures

## Test coverage
Each validation mode (`auto`, `callback`, `ddl`, `none`) will have its own test class:
- Each class validates the mode-specific behavior:
    - Persistence success/failure for valid/invalid entities
    - Schema structure (e.g., `NOT NULL` constraint presence)
    - `ConstraintViolationException` propagation in reactive flows
    - Enforcement of database constraints verified by attempting inserts using direct SQL queries

### Impact on test suites and testing automation
The existing `sql-db/hibernate-reactive` module will be enhanced with four additional test classes for each validation mode.

### Impact on resources
The expected execution time will increase by several minutes in JVM and Native mode for bare metal and OpenShift.

## Contacts
* Tester: Georgii Troitskii <gtroitsk@redhat.com>

## References
- https://quarkus.io/guides/hibernate-orm#validator_integration
