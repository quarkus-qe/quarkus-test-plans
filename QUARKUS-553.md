# QUARKUS-553 - OIDC / OAuth2 Service Account Authenticated REST Client

JIRA link: https://issues.redhat.com/browse/QUARKUS-553
GitHub link: https://github.com/quarkusio/quarkus/issues/5657

For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing

This feature adds three new Quarkus extensions: 
- OIDC Client to expose the OIDC client to be used by services.
- OIDC Client Filter to customize the OIDC filter use in HTTP services.
- OIDC Token Propagation to configure the token provided by the OIDC Client with the OIDC quarkus extension. 

As part of the above extensions development, the following test coverage has been implemented:
 - OIDC Client using `client_credentials` grant.
 - OIDC Client using `password` grant.
 - OIDC Client using `client_secret_post` configuration.
 - OIDC Client using `client_secret_jwt` configuration.
 - OIDC Client using a private PEM configuration.
 - OIDC Client using a private keystore configuration.
 - OIDC Client Custom Filter using lazy tokens acquisition.
 - OIDC Token Propagation using the `AccessToken` annotation.

 Therefore, the scope of the testing should ensure the compatibility with downstream endpoints such as the `RestClient` client and also verify the integration with the RH SSO versions.

### Impact on testsuites and testing automation:
 - Smoke tests to be added into the OpenShift TS:
   - Scenario with `OIDC Client` using RH SSO 7.3 and RH SSO 7.4.
   - Scenario with multitenant configuration using `OIDC Clients` new framework.
 - For integration components, more test development needs to be added in Beefy-scenarios testsuite:
   - Integration with [Rest Client](https://quarkus.io/guides/rest-client) Quarkus extension
 - Ensure this coverage works both in JVM and NATIVE mode

### Impact on resources:
 - More services will be running on OpenShift cluster, impact on CPU/memory should be minimal
 - Test execution will be prolonged, preliminarily estimation is ~30 minutes per each OCP stream
 - QUARKUS-553 covers both JVM and NATIVE mode, Native image imposes multiple times prolonged execution time and increased memory requirements for the build

Our current testing capacity is sufficient to cover this testing.

If multiple streams get introduced we need to have increased capacity in the lab.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use
 
Activity in this regard already lead to discussions - e.g. identified need for configuring the security configuration at runtime

## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage

## Advanced topics for test development
The following advanced topics are listed for future consideration. There should be dedicated feature requirements and/or enhancements for these specific areas.
 - Integration testing with more components ([gRPC](https://quarkus.io/guides/grpc), WebSockets, ...)
 - Backward compatibility testing
