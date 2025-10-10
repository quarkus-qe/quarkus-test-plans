# QUARKUS-6473 - Support runtime programmatic set up for basic and form-based authentication using fluent API

JIRA: https://issues.redhat.com/browse/QUARKUS-6473

Support runtime programmatic set up for basic and form-based authentication using fluent API

Support scope: 
 - Developer Preview - https://access.redhat.com/support/offerings/devpreview

Upstream PR:
- https://github.com/quarkusio/quarkus/pull/48659

Upstream documentation:
- https://quarkus.io/guides/security-basic-authentication#basic-auth-programmatic-set-up
- https://quarkus.io/guides/security-authentication-mechanisms#form-based-auth-programmatic-set-up
- https://quarkus.io/guides/security-authorize-web-endpoints-reference#programmatic-set-up-references

## Scope of the testing
QE team will:
- Get familiar with the functionality
- Assess test coverage provided as part of the PR #48659
- Focus on Basic Authentication as that's the default fallback
  - OIDC is the suggested approach for production setup
  - Form Authentication coverage is deferred to the upstream test suite
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
  - Pre-selected modules are `security/jpa` and `spring/spring-data`
  - Their configuration of basic authorization will be done programmatically
  - Experiments with `security/form-authn` module are optional
- Adjustments in the TS will also cover:
  - Scenario with joint usage of programmatic and declarative approaches
  - Scenario with OpenAPI descriptors of the endpoints using Basic Authentication
  - Multiple classes with `@Observes HttpSecurity httpSecurity` methods

### Impact on test suites and testing automation
The existing `security` modules will be enhanced, with no impact on automation or test suite layout.

### Impact on resources
No impact is anticipated for execution time and CPU/RAM resources.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## References
- https://quarkus.io/guides/security-authorize-web-endpoints-reference
