# QUARKUS-1398 - Test and verify GraphQL support

JIRA link: https://issues.redhat.com/browse/QUARKUS-1398

Coverage should ensure full GraphQL support from client and server side.
For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing
Quarkus supported GraphQL for a long time as a tech preview. At the moment, we only employ smoke tests and error validation,  but since we move towards full support, following actions are needed to ensure proper test coverage:

- Setup scenarios in order to cover reactive use cases, including working with Resteasy, Hibernate and Messaging.
- Coverage for @Blocking/@NonBlocking annotations
- Coverage for OpenTracing and MicroProfile Metrics
- Coverage for SmallRye GraphQL API(https://github.com/smallrye/smallrye-graphql)
- Coverage for WebSocket integration
- Low priority: Dev UI tests
- Nice to have: draft coverage(with disabled tests) for OpenTelemetry and Micrometer

### Impact on testsuites and testing automation:
- Ensure this coverage works both in JVM and Native mode

### Impact on resources:
 - More services will be running on OpenShift cluster, impact on CPU/memory should be minimal
 
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use
 
## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage
