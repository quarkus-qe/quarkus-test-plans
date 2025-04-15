# Quarkus-5622 Support @PermissionAllowed for @BeanParam parameter

Jira: https://issues.redhat.com/browse/QUARKUS-5622

Upstream PR: https://github.com/quarkusio/quarkus/pull/43353/ that close the issue https://github.com/quarkusio/quarkus/issues/43231  

## Scope of testing

 Verifying the functionality that allows Quarkus to pass specific fields from `@BeanParam` parameters into custom `java.security.Permission` constructor 
parameters.
The testing will focus on:
- Verify that specific fields from `@BeanParam` can be correctly referenced and passed to custom permission constructors.
- Ensure permissions constructor parameters are matched based on secured method parameter names.
- Test nested structures with multiple levels of depth (e.g., accessing fields like `beanParam.nestedObject.subObject.field` where objects are nested inside each other).
- Test different field access patterns including public fields, traditional getters, and record-style accessors (without "get" prefix).
- Verify the integration with other security mechanisms and annotations.
- Verify error conditions to ensure appropriate error handling.

### Test cases

- Verify that a user with the correct role can access resources protected with permissions using `@BeanParam` fields.
- Verify that access is denied when `@BeanParam` field values don't satisfy permission requirements.
- Verify functionality with `@BeanParam` objects containing multiple fields.
- Verify support for referencing nested properties in permission checks.
- Verify is possible to extract the same field from different `@BeanParam` types.
- Test functionality with Basic Auth and JWT.
- Test with missing or null `@BeanParam` values.

## Getting familiar with the feature
Getting familiar with the guide:
   - https://quarkus.io/guides/security-authorize-web-endpoints-reference
   - https://quarkus.io/guides/security-architecture
## Existing test coverage
QE TS already has tests for security authorization in `/quarkus-test-suite/security`

### Impact on test suite
New classes and tests will be created into `/quarkus-test-suite/security/permissions`.

## Impact on resources
Tests will be executed on both JVM and native and on both baremetal and OCP.
Expected additional execution time in minutes for each case.

## Contacts
* Tester: Jose Carranza <jcarranz@redhat.com>