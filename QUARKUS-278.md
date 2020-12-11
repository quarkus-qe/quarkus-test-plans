# QUARKUS-278 - Test and verify Integration with RH-SSO 7.4

JIRA link: https://issues.redhat.com/browse/QUARKUS-278

Coverage should ensure the integration with RH-SSO 7.4 and Quarkus security features.
For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing
Following actions were taken to ensure test coverage and automation for native image:
 - Define the scope of the testing
 - Analyze the possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
 - Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources
 - Check a need for involvement of other product/component teams

### Impact on testsuites and testing automation:
 - OpenShift TS framework needs to be enhanced to install RH-SSO 7.4 and/or other versions
 - For Quarkus 1.7, focused test development targets baseline test coverage into OpenShift TS:
   - Protect Services Applications with Roles
   - Protect Services Applications with Roles using permissions managed by RH SSO
 - For Quarkus 1.11, future test development will add the following test coverage into OpenShift TS:
   - Protect Web Applications with Roles
   - Protect Multi Tenant applications
   - Protect Service Applications using JWT
   - Cover Expiration of Bearer Token for Services Applications
   - User Revocation for Services Applications
 - For integration components, more test development needs to be added in Beefy-scenarios testsuite:
   - Integration between [OIDC](https://quarkus.io/guides/security-openid-connect) and [Rest Client](https://quarkus.io/guides/rest-client) Quarkus extensions
 - Ensure this coverage works both in JVM and NATIVE mode

### Impact on resources:
 - More services will be running on OpenShift cluster, impact on CPU/memory should be minimal
 - Test execution will be prolonged, preliminarily estimation is ~30 minutes per each OCP stream
 - QUARKUS-278 covers both JVM and NATIVE mode, Native image imposes multiple times prolonged execution time and increased memory requirements for the build

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
 - Integration testing with other components (gRPC, WebSockets, RestClient, ...)
 - Backward compatibility testing
