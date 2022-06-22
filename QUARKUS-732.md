# QUARKUS-732 - Reduce configuration when deploying to OpenShift

The goal of this test plan is to cover usage of Service Bindings[5] for managed Kafka on OpenShift.

JIRA link: https://issues.redhat.com/browse/QUARKUS-732

### Scope of the testing
 - Verify, that following properties can be replaced with Service Bindings
   - `%prod.kafka.bootstrap.servers`
   - `%prod.kafka.security.protocol`
   - `%prod.kafka.sasl.mechanism`
   - `%prod.kafka.sasl.jaas.config`
   - `%prod.quarkus.openshift.env.secrets`
   - `quarkus.openshift.env.mapping.kafka-username.from-secret`
   - `quarkus.openshift.env.mapping.kafka-username.with-key`
   - `quarkus.openshift.env.mapping.kafka-password.from-secret`
   - `quarkus.openshift.env.mapping.kafka-password.with-key`

### Impact on test suites and testing automation
- Additional module in the test suite for OpenShift testing
- These tests require deployment of Managed Kafka on OpenShift. This is not possible at this time, but we expect it to be implemented later( see also [7]).

### Impact on resources:
- Resource consumption of the new module and new tests should be roughly the same as for other OCP modules
- Managed Kafka on OpenShift will be consuming some cluster resources.

### Additional links
- [1] https://github.com/quarkusio/quarkus/pull/15733
- [2] https://github.com/quarkusio/quarkus/pull/18066
- [3] https://github.com/quarkusio/quarkus/pull/21618
- [4] https://github.com/smallrye/smallrye-reactive-messaging/pull/1092
- [5] https://quarkus.io/guides/deploying-to-kubernetes#service_binding
- [6] Test plans for Service Bindings in Quarkus: https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-1414.md
- [7] https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-1027.md#future-considerations 
