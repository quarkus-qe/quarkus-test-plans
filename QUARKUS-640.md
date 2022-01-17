# QUARKUS-640 - Testing and verifying OpenShift Kafka with Quarkus

The goal of this test plan is to cover the main points of conflicts between Quarkus and Kafka. 

JIRA link: https://issues.redhat.com/browse/QUARKUS-640

## Scope of the testing
Following actions were taken to ensure test coverage and automation for reactive kafka applications:
 - Kafka producer with JSON / AVRO body messages.
 - Kafka consumer with JSON / AVRO body messages.
 - Kafka Streams with JSON body messages and windowed.
 - Tested over Kafka Strimzi and RH AMQ Streams.
 - Platform OpenShift, Java 11.
 - Apicurio serdes (serializers/deserializers)

### Impact on testsuites and testing automation:
 - OpenShift TS framework needs to be enhanced to support Kafka on OpenShift
   - Would be great if in the future we support some kind of openshift/k8s templating. So install some specifict Kafka (provider/version) would be as simple as apply a template. 

### Impact on resources:
 - 1 Kafka broker (without replication) (755.5 MiB - 343.5 MiB)
 - 1 zookeeper () (61.8 MiB)
 - 1 Kafka registry (128Mb)

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Automated test development

Two scenarios has been setup:
- [KStreams Scenario](https://github.com/quarkus-qe/quarkus-openshift-test-suite/tree/main/messaging/kafka-streams-reactive-messaging): There is an EventsProducer that generate login status events every 100ms. A Kafka stream called WindowedLoginDeniedStream will aggregate these events in fixed time windows of 3 seconds. So if the number of wrong access excess a threshold, then a new alert event is thrown. All aggregated events(not only unauthorized) are persisted.

- [Kafka AVRO Scenario](https://github.com/quarkus-qe/quarkus-openshift-test-suite/pull/140): There is an EventsProducer that generates stock price events every 1s. The events are typed by an AVRO schema.  
A Kafka consumer will read these events serialized by AVRO and change a `status` property to `COMPLETED`. 
The streams of completed events will be exposed through an SSE endpoint.

## Advanced topics for test development
The following advanced topics are listed for future consideration. There should be dedicated feature requirements and/or enhancements for these specific areas.
 - Security testing
 - Recovery/Failover testing
 - JVM Native mode