# QUARKUS-278 - Test and verify Quarkus HTTP/2 support

JIRA link: https://issues.redhat.com/browse/QUARKUS-279

Coverage should ensure full HTTP/2 support from client and server side.
For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing
Quarkus support by default HTTP/2 since version 1.4 (https://quarkus.io/blog/quarkus-1-4-final-released/), so the following action were taken to ensure test coverage:

Client Side:

- Setup some scenarios in order to cover `GRPC` 
- Consume an HTTP/2 endpoint through `quarkus-rest-client`
- Consume an HTTP/2 endpoint through `quarkus-rest-client-mutiny`

Server Side:
- Verify that http/2 is set as default HTTP protocol
- Consume and endpoint developed with `quarkus-resteasy-jsonb` through HTTP/2 Client (Vertx HTTP Client)

### Impact on testsuites and testing automation:


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

## Advanced topics for test development
In the future we should extends this test to databases that support HTTP/2 as:
 - Elasticsearch Rest Client
 - `quarkus-mongodb-rest-data-panache`
 - `quarkus-hibernate-orm-rest-data-panache`
