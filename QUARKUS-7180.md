# QUARKUS-7180 - Support security annotations on Jakarta Data repositories

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7180

Upstream PR: https://github.com/quarkusio/quarkus/pull/50987

Quarkus adds support for securing Jakarta Data repository methods with security annotations.

Previously, security annotations placed on Jakarta Data repository interfaces or repository methods could be ignored. This caused annotations such as `@RolesAllowed`, `@Authenticated`, `@PermissionsAllowed` and `@DenyAll` to not be enforced on repository methods.

This feature adds support for securing Jakarta Data repository with security annotations. The upstream documentation also describes limitations for generic repository methods using type variables or wildcards.

## Scope of the testing
The testing will focus on validating that security annotations are correctly enforced on Jakarta Data repository interfaces and repository methods.

The following scenarios will be covered: 

* Test method level security annotations on Jakarta Data repository methods with:
    * `@RolesAllowed` - Verify methods annotated with `@RolesAllowed("admin")` allows access only for "admin" users and returns HTTP 403 for non admin users.

    * `@Authenticated` - Verify authenticated users can invoke repository methods secured with `@Authenticated` successfully while unauthenticated users receive HTTP 401.

    * `@PermissionsAllowed` - Verify users with the required permission can invoke repository methods secured with `@PermissionsAllowed` successfully while users with incorrect permissions receive HTTP 403.
        * Tests will include both single permission scenarios and multiple permissions required scenarios.

    * `@DenyAll` - All users are denied from using this repository method.

* Test class/type level security annotations on Jakarta Data repository interfaces:
    * Security annotations will be placed on the repository interface instead of individual repository methods.
    * Repository methods declared inside this interface should inherit the security rules and require the expected role, permission or authentication before execution.
    * The same annotations will be tested on the interfaces: `@RolesAllowed`, `@Authenticated`, `@PermissionsAllowed`, `@DenyAll`.

* Test combinations of annotations when there are security annotations on both repository interface level and method level.

* Test method-level and interface annotation precedence.

* Test repository inheritance scenarios:
    * Test method overriding in repository inheritance - verify expected behaviour when a child repository overrides the parent repository method with different security annotations.   
    * Test inherited method-level annotations.
    * Test inherited interface-level annotations.

* Test workaround for generics/wildcards as specified in the `hibernate-orm` documentation on Jakarta Data.

## Existing test coverage

Existing upstream tests are located here: https://github.com/quarkusio/quarkus/blob/main/integration-tests/hibernate-orm-data/src/test/java/io/quarkus/it/hibernate/processor/data/HibernateOrmDataSecurityTest.java


* Upstream test coverage include both method-level and class-level repository security testing using `@RolesAllowed`, `@Authenticated`, `@DenyAll`, `@PermissionsAllowed`.

* Overloaded repository methods - when 2 methods have the same name, the one with security annotation is secured and the other is not.

* Upstream tests also cover tests that contain generics in method parameters.

* The QE test suite currently does not contain any tests for this new feature. 

### Impact on test suites and testing automation

* Tests will be added to the `hibernate/jakarta-data` module of the Quarkus QE test suite. 
    
* Link: https://github.com/quarkus-qe/quarkus-test-suite/tree/main/hibernate/jakarta-data

* A new security test package will be added inside this module.

### Impact on resources

The added tests should have low impact on resources.

Execution time increase should be small since scenarios require only:
* Using multiple small databases of the same size.
* Invoking REST endpoints.
* Validating security annotations results.

Estimated execution time increase is expected to be less than 5 minutes.    

Tests are planned to execute on JVM, Native and OpenShift environments.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream issue and pull request.
* Reviewed upstream test coverage in `integration-tests/hibernate-orm-data`.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References

* https://github.com/quarkusio/quarkus/issues/48884
* https://quarkus.io/guides/hibernate-orm#secure-jakarta-data-repositories