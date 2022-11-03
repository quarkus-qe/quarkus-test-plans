# QUARKUS-2489 OIDC Back channel logout

Jira: https://issues.redhat.com/browse/QUARKUS-2489

This feature provides Back-Channel Logout capability for OIDC extension.

## Scope of the testing

Backchannel logout applies to `quarkus-oidc` and basically is a standard way to logout the current user from all the applications this user is currently logged in, bypassing the user agent.

We will create a valid token and proceed with the logout flow.
### Impact on test suites and testing automation

We will need to cover JVM and native mode in order to double-check that there are no conflicts with the native compilations. New Keycloak configuration id need it with all the logout flow parameters. 
### Impact on resources:

The new scenario is going to run in JVM and native mode over baremetal and OpenShift. So we are expecting about 5 min per module on JVM mode, and 10 min per module on native mode. Running a test over OpenShift used to take some extra time but is just an aproximation. To sum up, we are expecting an increase of 30 min of total amount execution.
## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides a clear explanation of expected behavior
- Review upstream and quickstarts coverage
- Read OpenID Connect specifications
- Manual testing

## Contacts

* Tester: Pablo Gonzalez Granados <pagonzal@redhat.com>

## References

- [Quarkus OpenID Connect documentation](https://quarkus.io/guides/security-openid-connect-web-authentication#back-channel-logout)
- [OpenID Connect 1.0 spec](https://openid.net/specs/openid-connect-backchannel-1_0.html#Backchannel)
- [OIDC logout PR](https://github.com/quarkusio/quarkus/pull/24611)
- [Keycloak logout](https://www.keycloak.org/docs/latest/server_admin/index.html#_oidc-logout)
  