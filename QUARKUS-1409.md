# QUARKUS-1409 - Add Cloud Events support to the AMQP connector

JIRA link: https://issues.redhat.com/browse/QUARKUS-1409

This test plan talks about cloudevents integration into AMQP connector

## External resources

 - [Implement Cloud Event support for the AMQP connector]([QUARKUS-728](https://github.com/smallrye/smallrye-reactive-messaging/pull/1517))
 - [Add Cloud Event support to the AMQP connector](https://github.com/quarkusio/quarkus/issues/21435)
 - [Kafka cloud events as an example](https://quarkus.io/blog/kafka-cloud-events/)
 - [Cloudevents](https://cloudevents.io/)
  
## Scope of the testing

Cloud Event proposes a common way to describe events in order to improve the interoperability between several systems. An event will have a common data structure that could be serialized/deserialized in JSON or binary format. 

Test development will cover AMQP producer and consumers of JSON and binary events.

### Impact on testsuites and testing automation:

We will need to create two new modules into `messaging` folder in order to cover JSON and binary serialization. The main impact is going to be time execution, that will be increased in 2 min to JVM mode and 4 min to native mode. 

AMQP over OCP is already covered in [QUARKUS-959](QUARKUS-959) and we don't think that this protocol add any extra case to the existing coverage, in terms of OCP.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Exploratory testing
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>