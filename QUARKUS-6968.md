# QUARKUS-6968 - Upgrade to Hibernate Validator 9.1

JIRA: https://issues.redhat.com/browse/QUARKUS-6968

Upstream PR: https://github.com/quarkusio/quarkus/pull/50196

## Scope of changes in Hibernate Validator 9.1
Hibernate Validator 9.1 is a minor update focused mainly on internal performance improvements and new functional additions.

The main areas affected are:
1. **Cascaded validation processing**  
    The update reworks how already-validated beans are tracked during cascading to reduce redundancy and boost performance.
    We need to verify that cyclic object graphs are still handled correctly, ensuring there are no missed violations, duplicates, or infinite recursion issues.

2. **`@Valid` annotation semantics**  
   Hibernate Validator 9.1 supports both the legacy container-level usage (`@Valid List<T>`) and the modern type-use style (`List<@Valid T>`). 
   We must confirm that both approaches are fully functional.

3. **Extended validation path**  
   Hibernate Validator 9.1 extends the internal validation path API to provide more detailed information about constraint locations.  
   This is an advanced API and is not plan to be tested, as applications typically rely only on standard constraint violation messages and property paths.

4. **`@IpAddress` constraint**  
   The new `@IpAddress` constraint validates that the corresponding string is a well-formed IP address

Testing will focus on validating application behavior rather than internal APIs or performance-related changes.

## Test coverage

### Cascading validation with cycles
- Add integration tests covering cascaded validation on object graphs with cyclic relationships.
- Verify that:
    - valid cyclic object graphs are persisted successfully
    - invalid cyclic object graphs fail with a validation error
    - validation terminates correctly without infinite loops

### Container-level vs type-use `@Valid` annotation
- Add tests covering both legacy container-level and type-use `@Valid`
- Verify that:
    - both approaches fail for invalid nested elements
    - both approaches succeed for valid input
    - validation results and reported paths are equivalent

### `@IpAddress` constraint
- Use a simple test entity with an IP address field.
- Verify that:
    - persisting an entity with an invalid IP address fails validation
    - persisting an entity with a valid IP address succeeds

## Impact on test suites and testing automation
The `hibernate/hibernate-reactive` module will be extended with:
- adding new entities for testing cycles in cascaded validation, container element constraints with type use `@Valid` annotations and the new `@IpAddress` constraint
- extend the existing `/validation` endpoint with additional validation scenarios
- adding new tests to existing test classes

## Impact on resources
The added tests extend existing test coverage and are expected to have a minimal impact on execution time.

## Getting familiar with the features
- Review Hibernate Validator 9.1 release notes and performance changes relevant to cascaded validation.
- Focus on user-visible behavior changes affecting validation with cycles, container level, type use `@Valid`, and the new `@IpAddress` constraint in Quarkus applications.

## Contacts
* Tester: Georgii Troitskii <gtroitsk@ibm.com>

## References

- [Hibernate Validator 9.1: release note](https://in.relation.to/2025/11/07/hibernate-validator-9-1-0-Final/)
- [Hibernate Validator 9.1: performance](https://in.relation.to/2025/09/29/hibernate-validator-benchmark/)
- [Hibernate – cascaded validation](https://docs.hibernate.org/stable/validator/reference/en-US/html_single/#_cascaded_validation)
- [Jakarta Bean Validation – container element constraints](https://jakarta.ee/specifications/bean-validation/3.1/jakarta-validation-spec-3.1#constraintdeclarationvalidationprocess-containerelementconstraints)