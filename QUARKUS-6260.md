# QUARKUS-6260 - Add OIDC bearer token step up authentication

JIRA: https://issues.redhat.com/browse/QUARKUS-6260

Upstream issues: https://github.com/quarkusio/quarkus/issues/46213

Upstream PR: https://github.com/quarkusio/quarkus/pull/47445

### Scope of the testing

#### The '@AuthenticationContext' annotation behavior

- ACR values requirement validation (Single, Multiple ACR values)
- MaxAge parameter validation
- Combined usage of ACR values and MaxAge validation
- Build should fail with proactive authentication enabled
- Build should fail if annotation is used with empty ACR values

#### Authentication flow scenarios

- **ACR & `maxAge` Policy Enforcement:**
  - Test with tokens that fully satisfy, partially satisfy, and do not satisfy the ACR requirements.
  - Test with tokens that are within and outside the specified `maxAge` limit.
  - Verify the fallback from `auth_time` to the `iat` claim for `maxAge` validation.
- **Challenge Response:**
  - Confirm a correct step-up challenge (`insufficient_user_authentication`) is sent when policy requirements are not met.
- **Feature Interactions:**
  - Interaction with RBAC (`@RolesAllowed`).
  - Usage with WebSockets Next endpoints.
  - Usage in Multi-tenant scenarios (via `@Tenant` and configuration).
  - Custom validators (`Jose4J`)
  - Behavior with expired tokens
  - Behavior with malformed ACR claims

### Existing test coverage

* **Upstream Test coverage:** was implemented in the PR #47445 - https://github.com/quarkusio/quarkus/pull/47445/files 

* **QE Test Suite:** There are no tests yet covered in our test suite. Then, new tests will be implemented and enhance the existing upstream coverage by adding
  scenarios and tests based on the scope described above in the existing `security/keycloak-oidc-client-extended` module.
 Tests will be executed on both JVM and native modes on both bare-metal and OCP.

### Impact on resources

The expected additional execution time will be increased about 2 minutes for JVM mode and 5 minutes for native mode.
On OCP, the execution time is expected to increase by around 6 minutes for JVM mode and 12 minutes for native mode.

### Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Review of the PR implementation and test coverage
- Review rfc9470 

### References

- https://quarkus.io/guides/security-oidc-bearer-token-authentication
- https://www.rfc-editor.org/rfc/rfc9470.html

### Contacts

* Tester: Jose Carranza <jcarranz@redhat.com>