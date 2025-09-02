# QUARKUS-6263 - Add OIDC Health Check

JIRA: https://issues.redhat.com/browse/QUARKUS-6263

Upstream issues: https://github.com/quarkusio/quarkus/issues/47861

Upstream PR: https://github.com/quarkusio/quarkus/pull/47830

## Scope of the testing

- OIDC Health Check is correctly registered when `quarkus.oidc.health.enabled=true` and `quarkus-smallrye-health` dependency is present
- OIDC Health Check is not registered if `quarkus.oidc.health.enabled=false`
- Individual tenant status testing:
    - **OK**: When OIDC provider is available 
    - **Disabled**: When `quarkus.oidc.tenant-enabled=false`
    - **Unknown**: When discovery is disabled and no discovery URI is configured
    - **Error**: When provider returns errors or is unreachable (stopping container)

- Multi-tenant health check data verification:
    - Verify the `data` field contains status for ALL configured tenants
    - Test scenarios with mixed tenant statuses (e.g., tenant-1: OK, tenant-2: Disabled, tenant-3: Error)
    - Confirm overall status is UP when at least one tenant is OK, DOWN otherwise
    - Validate that even when overall status is UP, the data field accurately reports problematic tenants
- Health check behavior:
    - Application remains operational when OIDC provider is unavailable
    - Health check endpoint accessibility on standard HTTP port (`/q/health/ready`)
    - Management Interface integration when configured


## Existing test coverage

* **Upstream Test coverage:** was implemented in the PR (#47830 - https://github.com/quarkusio/quarkus/pull/47830/files) includes tests validate the fundamental behaviors, such as the registration of the health check is UP and OK tenant status,
and also WireMock to verify other tenant statuses.

* **QE Test Suite:** There are no tests yet covered in our test suite. Then, new tests will be implemented and enhance the existing upstream coverage by adding 
multitenant scenarios and tests based on the scope described above that will be executed on both JVM and native modes on both bare-metal and OCP.

### Impact on test suite

A new test class `OidcHealthCheckIT` will be added to the `security/keycloak-oidc-client-extended` module.

### Impact on resources

The expected additional execution time will be increased in about 4 minutes for JVM mode and 10 minutes for native mode.
On OCP, the execution time is expected to increase by around 8 minutes for JVM mode and 20 minutes for native mode.

## Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Review of the PR implementation and test coverage
- Analysis of the integration between OIDC and SmallRye Health
- Understanding of different tenant states

## References

- https://quarkus.io/guides/security-oidc-expanded-configuration#create-oidc-configuration-at-request-time
- https://quarkus.io/guides/smallrye-health
- 
## Contacts

* Tester: Jose Carranza <jcarranz@redhat.com>