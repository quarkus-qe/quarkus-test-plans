# QUARKUS-5611 Support Keycloak Dev Service when OIDC client is used without OIDC extension
 
JIRA: https://issues.redhat.com/browse/QUARKUS-5611
 
Support Keycloak Dev Service when OIDC client is used without OIDC extension

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/43609

Upsteram issue:
- https://github.com/quarkusio/quarkus/issues/43086

## Scope of testing
- Testing the Dev Service for `quarkus-oidc-client` extension without `quarkus-oidc`.
- Tests will cover Dev Service user experience.
- In addition there will be simple tests to check that oidc-client can get token from OIDC server in Dev Mode.
- All test will use Keycloak as OIDC provider.

## Existing test coverage
- Partially covered using [`quarkus-extensions-combinations`](https://github.com/quarkus-qe/quarkus-extensions-combinations)

## Automated test development
- Test suite
  - Adding new module similar to [security/keycloak](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security/keycloak).
  - This module will contain the `quarkus-oidc-client` extension without `quarkus-oidc`.

### Impact on TS and resources
A new security module will be implemented.
This addition will have minimal impact on execution time, as it does not involve native or OCP execution.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
