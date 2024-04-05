# QUARKUS-2391 Promote the OpenTelemetry extension from TP to full support
JIRA:https://issues.redhat.com/browse/QUARKUS-2391
## Scope of the testing
Quarkus already supported Opentelemetry as a tech preview, but now we move towards full support, then we will extend our test coverage.
### Existing tests
In the current quarkus-test-suite exist two modules dedicated to opentelemetry in the monitoring folder, they are:

- opentelemetry [monitoring-opentelemetry]
- opentelemetry-reactive [monitoring-opentelemetry-reactive]

These tests utilize trace export to Jaeger (all-in-one pod) components and context propagation through a Ping-Pong application with 3 pods (ping service,pong service, jaeger-all-in-one).
The traces are directly sent to the Jaeger collector, and they also test Quarkus gRPC communication between services and SSE (Server-Sent Events).

On the other hand, also other modules use ```quarkus-opentelemetry``` dependency:
- javaee-like-getting-started
- build-time-analytics
- http/graphql-telemetry
- http/vertx-web-client
- sql-db/narayana-transactions (where is tested JDBC Opentelemetry instrumentation)

#### Test scenarios
- Disable Opentelemetry extensions to ensure the application behaves as expected without tracing enabled.
- Testing scenario with additional configuration options such as *Id Generator, Propagators, Resource, Sampler configuration*.
- Test with other exporters such a Zipkin.
- Integration tests to ensure proper tracing is enabled and functioning correctly in other core Quarkus extensions such as ```quarkus-smallrye-reactive-messaging-amqp```.
- Include gRPC reactive coverage to opentelemetry as we did with [opentracing](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/monitoring/opentracing-reactive-grpc) (recently dropped).

### Impact on testsuites and testing automation:
Some new tests will be added inside monitoring-opentelemetry modules, messaging-amqp-reactive.
On the other hand, a new module **open-telemetry-reactive-grpc** will be created, producing some time increase.

Ensure the test cases are running in both JVM and OpenShift environments.

## Getting familiar with the feature
To become familiar with the feature, the following actions were taken:
- Reviewed the guide and experimented with the [Opentelemetry-quickstart code](https://github.com/quarkusio/quarkus-quickstarts/tree/main/opentelemetry-quickstart)

## References
- [Opentelemetry-quickstart code](https://github.com/quarkusio/quarkus-quickstarts/tree/main/opentelemetry-quickstart)
## Contact
- Tester: Jose Carranza  <jcarranz@redhat.com>