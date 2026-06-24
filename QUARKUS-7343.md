# QUARKUS-7343 - OIDC AuthenticationCompletionAction for authorization code flow

JIRA: https://issues.redhat.com/browse/QUARKUS-7343

Upstream PR: https://github.com/quarkusio/quarkus/pull/52537

Upstream issue: https://github.com/quarkusio/quarkus/issues/52536

PR #52537 introduces the `io.quarkus.oidc.AuthenticationCompletionAction` interface, which allows registering CDI beans that are executed exactly once upon successful authorization code flow completion, before the session cookie is created.
This is useful when you need an action to be completed before the current request is allowed to proceed, especially when an action requires a blocking call (e.g. provisioning a user in a database) must complete before the user's first request is served. 

## Scope of testing

Testing will focus on verifying `AuthenticationCompletionAction` integration using a real Keycloak instance, and a PostgreSQL database, covering tests to:

- Verify that a registered `AuthenticationCompletionAction` is invoked exactly during authorization code flow completion. The action provisions the authenticated user in a PostgreSQL database. Verify the
  database record exists after login, confirming the action executed before the HTTP response was sent to the user.
- Verify that subsequent requests from the same authenticated session do not trigger the action again.
- Verify that when an `AuthenticationCompletionAction` returns a failed `Uni`, the authentication fails and the user receives an error response.
- Verify that after a user's session expires and they re-authenticate via the code flow, the action is invoked again.
- Verify that when multiple `AuthenticationCompletionAction` beans are registered, all are executed upon code flow completion, and if one fails, the remaining actions are not executed.

## Impact on test suites and testing automation

New test classes will be added to the existing `security/keycloak-webapp` module.

## Impact on resources

- Estimated: ~1 min JVM, ~3 min native additional execution time.
- OCP adds the execution time is expected to increase by around 5 minutes for JVM mode and 8 minutes for native mode.

## Getting familiar with the feature

- Review upstream PR [#52537](https://github.com/quarkusio/quarkus/pull/52537) and linked issue [#52536](https://github.com/quarkusio/quarkus/issues/52536)
- Quarkus guide: [Authentication completion actions](https://quarkus.io/guides/security-oidc-code-flow-authentication#authentication-completion-action)

## Contacts

* Tester: Jose Carranza <jcarranz1@ibm.com>

## References

- https://quarkus.io/guides/security-oidc-code-flow-authentication#authentication-completion-action
- https://quarkus.io/guides/security-oidc-expanded-configuration
