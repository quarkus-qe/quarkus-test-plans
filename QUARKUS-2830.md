# QUARKUS-2829 - Full Support Micrometer metrics also when using managed Vert.x in Quarku

JIRA: https://issues.redhat.com/browse/QUARKUS-2830

As part of supporting Vert.x use-cases on Quarkus we need to support collecting metrics using micrometer for the managed Vert.x instance in Quarkus. This is also a requirement for some layered products like AMQ Streams and OpenShift Serverless that today are using Vert.x, but needs to move to use Quarkus.

## Scope  of the implementation
Managed Vert.x in Quarkus contains following extensions:
- Messaging clients:
  AMQP, Kafka, MQTT. Extension names: quarkus-smallrye-reactive-messaging-amqp, *-kafka, *-mqtt.
- DB clients: Mssql, DB2, Oracle, MySQL, Postgres(pg). Extension names: quarkus-reactive-(name)-client.
- Redis client: quarkus-redis-client
- quarkus-mailer
- quarkus-vertx (the core API)

In comparison to internal implementation (QUARKUS-2829), the following additions were made:
- mailer metrics
- grpc metrics
- database metrics (db2, mssql, mysql, oracle, postgresql, redis)
- TCP metrics (bytes_written, bytes_read, connections)
- UDP metrics (bytes_written, bytes_read, errors)
- EventBus metrics (eventBus.published, eventBus.sent, replyFailures, handlers, delivered, discarded)
- Http client metrics (queue.delay, queue.size)

## Scope of testing
Cover, that metrics are being created properly for applications, running on different targets (baremetal, openshift and serverless).

### Existing tests
Upstream tests contain coverage for Redis, HTTP client, telnet, eventbus, TCP and UDP metrics on baremetal.
As a part of QUARKUS-2829, we added checks for https server metrics on baremetal, openshift and serverless.

## Automated test development
We should add checks:
- for databases properties, preferably in vertx-sql module.
- for event-bus specific properties on different targets, vertx-http module seems to be a good target.
- for grpc. We need either add new endpoint into vertx-http or gather metrics in existing http-advanced/opentelemetry-reactive modules

### Impact on resources:

- No additional requirements for resources in lab
- Since we will not add new test classes, running time should change only slightly

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use


## References
- [JIRA issue](https://issues.redhat.com/browse/QUARKUS-2830)
- [Internal support issue](https://issues.redhat.com/browse/QUARKUS-2829)
- [Core guide](https://quarkus.io/guides/vertx)
- [Extensions guide](https://quarkus.io/guides/vertx-reference )
- [Event bus guide](https://quarkus.io/guides/reactive-event-bus)
