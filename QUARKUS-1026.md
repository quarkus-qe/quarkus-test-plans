# QUARKUS-1026 Continuous Testing in Dev Mode

JIRA link: https://issues.redhat.com/browse/QUARKUS-1026

Test plan covers Continuous Testing in Dev Mode functionality: https://quarkus.io/guides/continuous-testing

## Scope of the testing
- Enable/Disable Continuous Testing on a running Quarkus application
- Cover DEV UI functionality about continuous testing
- Detect new tests when continuous testing is enabled
- Run affected tests after new changes when continuous testing is enabled
- Dev services should be deployed/undeployed for tests (if required) when continuous testing is enabled
 - Kafka Dev services
 - Database Dev Services
 - AMQP Dev Services
 - (optional) Keycloak Dev Services
- Cover gRPC testing when continuous testing is enabled
 - gRPC is an special use case as it requires another server listening to a local port

### Impact on testsuites and testing automation:
- Enhance existing test suite / test framework
- Ensure this coverage works fine on DEV mode
- Ensure this coverage works fine on Unix and Windows environments (for coverage that does not need Docker as this is a limitation in Windows)

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Manual verification
- Ensure Continuous Testing is documented

## Contacts
* Tester: Jose Carvajal <jcarvaja@redhat.com>
