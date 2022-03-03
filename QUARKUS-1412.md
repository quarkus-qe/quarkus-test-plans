# QUARKUS-1412 - Integrate more advanced Kafka features

The goal of this test plan is to cover the main points of conflicts between Quarkus and Kafka. 

JIRA link: https://issues.redhat.com/browse/QUARKUS-1412

## Scope of the testing
 - Kafka producer and consumer with batch processing

### Impact on resources:
Negligible, just more cases for current tests 

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use


## Advanced topics for test development
The following advanced topics are listed for future consideration. There should be dedicated feature requirements and/or enhancements for these specific areas.
 - Checkpointing (when it is ready)
 - Rebalancer tests (https://smallrye.io/smallrye-reactive-messaging/3.14.1/kafka/receiving-kafka-records/#consumer-rebalance-listener)
