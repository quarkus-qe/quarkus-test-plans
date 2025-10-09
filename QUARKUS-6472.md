# QUARKUS-6472 - Provide a fluent API to set up path-specific authorization programmatically

JIRA: https://issues.redhat.com/browse/QUARKUS-6472

Provide a fluent API to set up path-specific authorization programmatically

Support scope: 
 - Developer Preview - https://access.redhat.com/support/offerings/devpreview

Upstream PR:
- https://github.com/quarkusio/quarkus/pull/48482

Upstream documentation:
- https://quarkus.io/guides/security-authorize-web-endpoints-reference#path-specific-authz-programmatic-set-up

## Scope of the testing
QE team will:
- Get familiar with the functionality
- Assess test coverage provided as part of the PR #48482
- Explore possible enhancements of the existing functionality and upstream test coverage
- Add new scenarios into QE TS

## Getting familiar with the feature
Getting familiar with
- Documentation and related PR
- Spring Boot way of doing programmatic configuration

## Test coverage
Upstream codebase
- Review of existing test coverage
  - Implementing missing test coverage
- Possible enhancements of the existing functionality
  - Implementation of the enhancements is optional

QE TS codebase
- Selected modules will be enhanced with a programmatic approach instead of definition in `application.properties`
  - Pre-selected modules are `security/https` and `security/keycloak-oidc-client-reactive-extended`
  - Their configuration of path-specific authorization will be done programmatically
- Adjustments in the TS will also cover:
  - Joint usage of programmatic and declarative approaches
  - Multiple methods with `@Observes HttpSecurity httpSecurity`

### Impact on test suites and testing automation
The existing `security` modules will be enhanced, with no impact on automation or test suite layout.

### Impact on resources
No impact is anticipated for execution time and CPU/RAM resources.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## References
- https://quarkus.io/guides/security-authorize-web-endpoints-reference
