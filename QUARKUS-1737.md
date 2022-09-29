# QUARKUS-1737 Reactive and Distributed systems roadmap

Jira: https://issues.redhat.com/browse/QUARKUS-1737

This test plan covers several changes, related to reactive and distributed systems and included in Quarkus Fireball release(2.13)

## Scope of the testing
Following topics should be covered:

1. Usage of upstream Kafka image(Strimzi) in addition to the Redpanda one in Kafka dev services.
2. Resteasy reactive now produces valid json array, when endpoint returns Multi.

### Impact on test suites and testing automation
- One new test class added for Kafka Dev Services
- One test method in sql/hibernate reactive refactored 


### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be less, than minute.

## Advanced topics for test development

- No advanced topics found

## References

- [the roadmap](https://issues.redhat.com/browse/QUARKUS-1737)
- [Kafka changes](https://issues.redhat.com/browse/QUARKUS-1411)
- [Bug fix for Multi and Json](https://issues.redhat.com/browse/QUARKUS-1220)
