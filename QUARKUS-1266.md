# QUARKUS-1266 Support Kafka AVRO
 - JIRA link: https://issues.redhat.com/browse/QUARKUS-1266
 - Quarkus Guide: https://quarkus.io/guides/kafka-schema-registry-avro
 - Apache Kafka AVRO Reference: http://avro.apache.org/docs/current/

## Scope of the testing
 - Extension must generate Java "whistles" or objects that represents the AVRO schemas in Java language
 - Extension must be able to serialize and deserialize AVRO events from/to Kafka
 - AVRO Schemas should be registered and versioned in an schema registry
 - Must work over Kafka Strimzi and Confluent
 - Consume and produce events in JVM and native mode

## Getting familiar with the feature
 - Focus on exploratory testing of the feature
 - Experiment with quickstart guide
   - https://github.com/quarkusio/quarkus-quickstarts/tree/main/kafka-avro-schema-quickstart


## Automated test development
 - New test to be added into Quarkus QE tests suite to cover scenario with `quarkus-avro` extension
