# QUARKUS-2829 - Internal support for Micrometer metrics when using managed Vert.x in Quarkus

JIRA: https://issues.redhat.com/browse/QUARKUS-2829


As part of supporting Vert.x use-cases on Quarkus we need to support collecting metrics using micrometer for the managed Vert.x instance in Quarkus. This is also a requirement for some layered products like AMQ Streams and OpenShift Serverless that today are using Vert.x, but needs to move to use Quarkus. For 2.13 the support for this is limited to OpenShift Serverless and AMQ Streams, this is not supported for general use. Support for general use should come in RHBQ 3.x

## Scope  of the implementation
Managed Vert.x in Quarkus contains following extensions:
- Messaging clients:
  AMQP,Kafka, MQTT. Extension names: quarkus-smallrye-reactive-messaging-amqp, *-kafka, *-mqtt.
- DB clients: Mssql, DB2, Oracle, MySQL, Postgres(pg). Extension names: quarkus-reactive-(name)-client.
- Redis client: quarkus-redis-client
- quarkus-mailer
- quarkus-vertx (the core API)

Current implementation provides following metrics (full names is `poolType+".pool."+MetricName`):
- queue.delay
- queue.size
- usage
- active
- idle
- ratio
- completed
- processing
- current
- reset
- pending
- connections

## Scope of testing
According to PM, the main focus of the current change is Serverless. Therefore, the optimal way is to cover only basic functionality for applications deployed in serverless mode.

### Existing tests
Upstream tests contain coverage for Redis, HTTP client, telnet, TCP and UDP metrics on baremetal.
[Quarkus start-stop test suite](https://github.com/quarkus-qe/quarkus-startstop) contains several modules, which contain Vertx.X based extensions, the most interesting for our current efforts are monitoring-micrometer-prometheus-kafka-reactive which cover gathering of Micrometer metrics from AMQ Streams applications.  

## Automated test development
New checks for Vert.X-specific metrics should be added into monitoring-micrometer-prometheus-kafka modules.
New module should be added, based on http-minimum, but with endpoints should be handled by Vert.X, and not resteasy. This module should check, that the application works and that metrics are collected and updated; it should handle baremetal, Openshift and Serverless use-cases.  

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Future considerations
Ideally (for Quarkus 3.0), we need to check, that we can get metrics for all supported extensions, and for working with Vert.X specific functionality of verticles and event bus.


## References
- [JIRA issue](https://issues.redhat.com/browse/QUARKUS-2829)
- [Core guide](https://quarkus.io/guides/vertx)
- [Extensions guide](https://quarkus.io/guides/vertx-reference )
- [Event bus guide](https://quarkus.io/guides/reactive-event-bus)
