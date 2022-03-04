# QUARKUS-1400 - Operator SDK product support (Tech preview)

JIRA link: https://issues.redhat.com/browse/QUARKUS-1400

General info: https://javaoperatorsdk.io/

Current development of the feature takes place in https://github.com/quarkiverse/quarkus-operator-sdk.
Documentation: https://quarkiverse.github.io/quarkiverse-docs/quarkus-operator-sdk/dev/index.html

Integration of Java Operator SDK with Quarkus.  
The Operator SDK provides the tools to build, test, and package Operators.  
The Java Operator SDK offers a way to do that from Java code using [fabric8](https://github.com/fabric8io/kubernetes-client) Kubernetes client.

## Scope of testing

### Existing test coverage
Integration tests in the extensions' repository:
- https://github.com/quarkiverse/quarkus-operator-sdk/tree/main/integration-tests/src/test/java/io/quarkiverse/operatorsdk/it

## Automated test development
Derive a usable test scenario from existing examples in https://github.com/quarkiverse/quarkus-operator-sdk/tree/main/samples.

## Impact on test suites and test environment
A new module in QE TS is expected, containing at least one OpenShift scenario. Impact on TS execution time would be at least 5 / 10 minutes in JVM / native mode.
