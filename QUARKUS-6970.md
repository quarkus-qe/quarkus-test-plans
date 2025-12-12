# QUARKUS-6970 OIDC token propagation

## Links to the resources
- JIRA ticket: https://issues.redhat.com/browse/QUARKUS-6970
- GitHub pull request related and the JIRA issue:  

    https://github.com/quarkusio/quarkus/pull/50723

    https://github.com/quarkusio/quarkus/issues/49878

## Scope of the testing

The [PR #50723](https://github.com/quarkusio/quarkus/pull/50723) introduces granular control over OIDC token propagation in REST Clients. Previously, the `@AccessToken` annotation could
only be applied at the interface level, forcing all methods within that client to propagate the token.

This enhancement allows the `@AccessToken` annotation to be applied to specific methods within a REST Client interface. This enables hybrid configuration scenarios where a single client
can have authenticated methods, unauthenticated methods, and methods that perform token exchange.

### What will be tested

- **Method-Level Standard Propagation** - Verify that a method annotated with `@AccessToken` successfully propagates the current incoming Bearer token to a secured backend.

- **Method-Level Token Exchange** - Verify that a method annotated with `@AccessToken(exchangeTokenClient = "target-client")` triggers the token exchange flow only for that specific call, 
sending a different token to the backend.

- **Mixed Configuration in Single Client** - Verify that a single REST Client interface (`TokenPropagationMethodPongClient`) can correctly isolate behaviors per method.    

- **Negative Scenarios**:
    - **No Propagation**: Verify that a method without the `@AccessToken` annotation does not propagate any token.
    This is confirmed by asserting that calling a secured endpoint via this unannotated method results in a `401` error.

The test coverage will be verified for JVM, native mode, and Openshift.

## Existing test coverage

- **Upstream Test coverage:** The PR includes `AccessTokenAnnotationTest.java` in the `extensions/oidc-token-propagation-reactive` module, covering method propagation and multi-client scenarios using mocks.

- **QE Test Suite:** The `keycloak-oidc-client-reactive-extended` module currently tests `@AccessToken` at the class level but there is no coverage
for method-level and the mixing of configurations.

### Impact on test suite

New test classes will be added to the `keycloak-oidc-client-reactive-extended` module to verify this feature using Keycloak.

### Impact on resources

The expected additional execution time will be increased approximately:
   - Some seconds for JVM mode and 1 minute for native mode on bare-metal
   - 1 minute for JVM mode and 2 minutes for native mode on OCP

The tests reuse the existing Keycloak container and OIDC infrastructure already running in the module.

## Getting familiar with the feature

- Check the specific PR: https://github.com/quarkusio/quarkus/pull/50723/
- Updated documentation section: https://quarkus.io/guides/security-openid-connect-client-reference#token-propagation-rest

## Community status of the extension/feature:
The extensions used in the module:
- quarkus-rest-client-oidc-token-propagation (tech-preview)
- quarkus-oidc (supported)

## Contacts
* Tester: Jose Carranza <jcarranz1@ibm.com>