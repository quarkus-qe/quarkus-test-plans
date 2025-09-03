# QUARKUS-6264 - Bump kafka-clients from 3.9.0 to 4.0.0

JIRA: https://issues.redhat.com/browse/QUARKUS-6264

Upstream PRs:
    - https://github.com/quarkusio/quarkus/pull/47108

This change bumps kafka-client dependencies to the new major version 4.0.0

## Getting familiar with the issue
In quarkus ecosystem kafka-clients is available indirectly via `quarkus-messaging-kafka` or`quarkus-kafka-client`.
Quarkus' changelog and migration guide does not state any new features or notable changes related to this bump.
Also, Kafka's [release announcement](https://kafka.apache.org/blog#apache_kafka_400_release_announcement),
[release notes](https://archive.apache.org/dist/kafka/4.0.0/RELEASE_NOTES.html) and
[upgrade guide](https://kafka.apache.org/40/documentation.html#upgrade) don't state changes significant for us.
For kafka-clients There are a lot of internal implementation changes, requirement for java 11, but very little changes that would be notable for users in Quarkus.

## Scope of testing
Main goal of this testing is to check for regressions related with this dependency update.
Since no notable changes are expected no new tests should be required.

Updating Kafka to the new major version should also be done in `quarkus-test-framework`, by bumping the Kafka broker.
This might lead to problems requiring changes in FW/TS to work correctly or expose problems with compatibility.

## Existing test coverage.
Our test suite already has significant test coverage in several `messaging/kafka-*` modules.
This coverage is sufficient for this testing.

## Impact on test suite and resources
None

## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>
