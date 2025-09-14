# QUARKUS-6262 - Upgrade to Hibernate Validator 9.0

JIRA: https://issues.redhat.com/browse/QUARKUS-6262

Upstream issues:

- https://github.com/quarkusio/quarkus/issues/5531
- https://github.com/quarkusio/quarkus/issues/35144

Upstream PR: https://github.com/quarkusio/quarkus/pull/42292

## Scope of changes in PR #42292
PR #42292 extends Hibernate Validator support in Quarkus, covering two areas:
1. Support for `validation.xml` constraints – although Quarkus does not support configuration of the `ValidatorFactory` via XML, constraint declarations in `validation.xml` must be respected.
2. Method override rules – validation must ensure that a method overriding another cannot redefine the parameter constraint configuration (as required by Bean Validation specification).

## Test coverage
### XML-based constraints
Add new tests to `AbstractHibernateValidatorIT` in `sql-db/hibernate-reactive` module:
- Create entity without annotations
- Define constraints in `META-INF/validation.xml`
- Tests should verify field constraints on valid and invalid entities

### Method override rules
Add new test class under `http/rest-client-reactive` module:
- Base service/interface defines a method parameter with constraints such as `@ConvertGroup`, `@NotNull`, and `@Valid`.
- Provide an implementation that overrides the parent method without redeclaring constraints.
- Tests:
  - Sending an invalid payload must fail with `ConstraintViolationException` (HTTP 400).
  - Sending a valid payload must succeed (HTTP 200).
  - Confirms that the constraints on the base method are inherited correctly, and no `HV000151` is raised at deployment.

## Impact on test suites and testing automation
The existing Hibernate Validator tests in `sql-db/hibernate-reactive` module will be enhanced with scenarios for XML-based constraints.

Method override rules will be verified `http/rest-client-reactive` by adding new test.

## Impact on resources
- **XML-based constraints**: the added tests will increase execution time by only a few seconds.
- **Method override rules**: the new test class will increase runtime by approximately 20–30 seconds. These tests are not executed on OpenShift.

## Getting familiar with the features
### XML-based constraints (`validation.xml`)
- [Hibernate Validator – Mapping constraints via XML](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#section-mapping-xml-constraints)
- [Quarkus – Bean Validation limitations](https://quarkus.io/guides/validation#limitations)
- Related upstream issue: [quarkusio/quarkus#5531](https://github.com/quarkusio/quarkus/issues/5531)

### Method override rules
- [Jakarta Bean Validation Specification – Method constraints](https://jakarta.ee/specifications/bean-validation/3.1/jakarta-validation-spec-3.1#constraintdeclarationvalidationprocess)
- [Hibernate Validator – Method and constructor constraints](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#chapter-method-constraints)
- Related upstream issue: [quarkusio/quarkus#35144](https://github.com/quarkusio/quarkus/issues/35144)

## Contacts
* Tester: Georgii Troitskii <gtroitsk@ibm.com>

## References

- [Hibernate Validator 9.0: What's new](https://hibernate.org/validator/releases/9.0/#whats-new)
- [Hibernate Validator - Mapping constraints via constraint-mappings defined in XML](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#section-mapping-xml-constraints)
- [Quarkus – Bean Validation limitations](https://quarkus.io/guides/validation#limitations)
- [Jakarta Bean Validation Specification – Method constraints](https://jakarta.ee/specifications/bean-validation/3.1/jakarta-validation-spec-3.1#constraintdeclarationvalidationprocess)
- [Hibernate Validator – Method and constructor constraints](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#chapter-method-constraints)