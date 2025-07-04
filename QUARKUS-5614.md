# QUARKUS-5614 Support `@PermissionsAllowed` defined on meta-annotation

JIRA: https://issues.redhat.com/browse/QUARKUS-5614

Support `@PermissionsAllowed` defined on meta-annotation

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/43241

Upsteram issue:
- https://github.com/quarkusio/quarkus/issues/39663

Documentation:
- https://quarkus.io/guides/security-authorize-web-endpoints-reference

## Scope of testing
Verify that `@PermissionsAllowed` works correctly when applied on a meta-annotation.
These tests will cover various scenarios of meta-annotations defined on both methods and classes.
Furthermore, the tests will validate the combined usage of directly applied `@PermissionsAllowed` annotations and those defined through meta-annotations on a single method, ensuring they operate together.
Additionally, the direct application of `@PermissionsAllowed` on methods and classes will be tested, as this is currently not covered by QE.

The tests will verify that `@PermissionsAllowed` works correctly when used with Quarkus REST (`quarkus-rest`).


## Automated test development

### Definition of roles and permissions

The tests will use 4 roles (for 4 users) and 4 defined permission.
Each role will be assigned a unique set of permissions.
These roles will be defined in property file using Elytron security.
The need for 4 roles and 4 permission is to validate multiple combination and definition.

### Test cases
- Both definition on meta-annotation and direct use of `@PermissionsAllowed` will have same cases, which should have same results.
- These cases are:
  - Two annotation on one method where each have one permission
  - Two annotation on one method where one have one permission and second have multiple permission
  - Two annotation on one method where one have one permission and second have multiple permission and defined inclusive (user role must have all permission)
  - One annotation with multiple permission
  - One annotation with multiple permission and defined inclusive
  - Permission with definition of own permission class
    - One annotation with multiple permission and custom permission class
    - One annotation with multiple permission, custom permission class and defined inclusive
  - Permission defined on class
    - Method without defined permission will use permission defined on class
    - Method without defined permission will use permission defined on class with defined inclusive
    - Method override class permission by own definition
- Testing direct use of `@PermissionsAllowed` and `@PermissionsAllowed` defined using meta-annotation together:
  -  Two annotation on one method where:
  - Direct usage have one permission and meta-annotation have multiple permission
  - Direct usage have one permission and meta-annotation have multiple permission with defined inclusive
  - Direct usage have multiple permissions and meta-annotation have one permission
  - Direct usage have multiple permissions with defined inclusive and meta-annotation have one permission
  - Permission inherited from definition in interface
    - Direct usage have one permission and meta-annotation have multiple permission
    - Direct usage have one permission and meta-annotation have multiple permission with defined inclusive


### Impact on TS and resources
The new module `security/permissions` will be added to QE TS.
This module execution will take around 30 seconds for JVM mode and 3 minutes for native mode.
For OpenShift the JVM mode will take around 8 minutes and native mode take 14 minutes.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
