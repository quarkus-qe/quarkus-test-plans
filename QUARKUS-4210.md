# QUARKUS-4592 - Hide LDAP hierarchy from developers by allowing mapped roles in the application code @RolesAllowed("localRole")

JIRA: https://issues.redhat.com/browse/QUARKUS-4210

Upstream issue: https://github.com/quarkusio/quarkus/issues/10264

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/39339
- https://github.com/quarkusio/quarkus/pull/45586

Our customer uses Elytron security LDAP extension and their application-specific `SecurityIdentity` roles differ between implemented applications.
LDAP group attributes can be mapped to the `SecurityIdentity` roles with the `quarkus.security.ldap.identity-mapping.attribute-mappings.*` configuration properties.
These roles can be mapped to the application-specific `SecurityIdentity` roles with the `quarkus.http.auth.roles-mapping` properties.
If role mapping must be dynamic, decided during an HTTP request processing, the `SecurityIdentityAugmentor` can be used instead.
LDAP group attributes are also added as `SecurityIdentity` attributes and the augmentor can be used to remap the attributes to roles.

## Scope of the testing
This feature is implemented in Vert.x HTTP extension and is applicable to every HTTP request processed by Vert.x HTTP server.
The reason why it works is that Elytron `IdentityProvider`s are enhanced after authentication has completed.
Every authorization method (HTTP permissions, standard security annotations) will work as long mapping is applied.

Our test coverage must verify:
- roles mapping is applied before HTTP permissions are checked
- roles mapping is applied before standard security annotations are checked
- native mode is supported
- smoke check for the actual customer scenario - map LDAP groups from the Elytron identity to application-specific roles

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
Elytron security extensions are not supported in product and Quarkus QE happen to test Elytron Security Properties File
mainly to simplify security testing focused on other features. We do not use LDAP server anywhere.
Upstream uses in-memory LDAP server managed by a resource from the `Quarkus - Test framework - embedded LDAP server` library.
Upstream test coverage includes test for a customer scenario.
The role mapping is tested extensively in the upstream test coverage.
Integration tests (native mode), HTTP permissions and standard security annotations are tested sufficiently.

### Impact on test suites and testing automation
No impact on test suites or testing automation.
The feature will be verified with the upstream test coverage.

### Impact on resources
No impact on resources.
The feature will be verified with the upstream test coverage.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [Quakrus documentation - Security LDAP - Map LDAP groups to SecurityIdentity roles](https://quarkus.io/version/main/guides/security-ldap#map-ldap-groups-to-securityidentity-roles)
- [Quarkus QE tracker](https://issues.redhat.com/browse/QQE-1277)