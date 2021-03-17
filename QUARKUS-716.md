# Quarkus-716 - Testing and verifying Quarkus OpenAPI schemas

The goal of this test plan is to cover Open API schemas generation.

JIRA link: https://issues.redhat.com/browse/QUARKUS-716 


## Scope of the testing
Following actions were taken to ensure test coverage and automation for OpenAPI schemas generation:
 - Setup and document a Rest API.
 - Validate generated Open API spec.

### Impact on testsuites and testing automation:
 - Ensure this coverage works both in JVM and NATIVE mode
 - Integration tests covered by beefy-scenarios testsuite 

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features

## Automated test development

- [Quarkus/OpenApi scenario](https://github.com/quarkus-qe/beefy-scenarios/tree/main/013-quarkus-oidc-restclient): Ping API will invoke Pong API using Quarkus Mutiny RestClient and returns the expected "ping pong" output. This API is annotated with OpenAPI tags. 
