# QUARKUS-1397 - Dev preview for Java 17 in Native mode

JIRA link: https://issues.redhat.com/browse/QUARKUS-1397

This test plan cover JDK17 Mandrel dev preview Quarkus integration.

## Related test plans

 - [QUARKUS-728](QUARKUS-728)

## Scope of the testing

Test development will focus on
- Current native scenarios must work with Java 17

### Impact on testsuites and testing automation:

- Native daily GitHub builds will cover Mandrel community JDK17 container image `ubi-quarkus-mandrel:21.3-java17`
- Mandrel image based on Java 17 (`quarkus/mandrel-21-jdk17-rhel8`) is in a technical preview stage and is going to be covered by extended platform testing as a subset of testing coverage.
- Native JDK17 will be covered by the following Jenkins jobs:
  - rhbq-2.7-extended-platform-testing-trigger
     - rhbq-2.7-baremetal-ts-native
- Native pipelines are expensive in terms of time execution and also in CPU and memory usage. The instance type that we are gonna use is xlarge and ideally, lab capacity should be increased in order to cover new pipelines that are gonna be created. 
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Exploratory testing as OCP native scenarios 
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>