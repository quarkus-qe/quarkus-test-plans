# QUARKUS-7057 SmallRye Reactive Messaging Kafka Request/Reply - Technology Preview

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7057

Upstream PR: https://github.com/quarkusio/quarkus/pull/52500

Upstream documentation:
* https://smallrye.io/smallrye-reactive-messaging/4.20.0/kafka/request-reply/
* https://quarkus.io/guides/kafka#kafka-request-reply

Kafka Request/Reply introduces a feature, allowing sender to send a message and await for the reply for that specific message. 

## Scope of testing
Request/Reply is a simple pattern, when one party sends a message into kafka and other party responds to that.
Tests will cover exchanging request-reply messages of basic data types and using smallrye `Message`.
Tests will also check default and custom value for reply channel.

## Existing test coverage
Quarkus QE TS has several tests for kafka, but none regarding the request/reply pattern.

## Impact on testsuite
New test class will be created in module `messaging/kafka-producer`.

## Impact on resources
New test class will be added. It will be run on JVM and native, on bare metal and OCP.
Expecting additional execution time of few minutes. 

## Contacts
* Tester: Martin Ocenas <mocenas@ibm.com>
