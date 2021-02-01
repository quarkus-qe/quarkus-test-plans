# Quarkus-712 - Rest Client support for Mutiny

The goal of this test plan is to cover Quarkus/Mutiny rest Client. 

JIRA link: https://issues.redhat.com/browse/QUARKUS-712 

## Scope of the testing
Following actions were taken to ensure test coverage and automation for Quarkus/Mutiny rest Client:
 - Setup async endpoints secured and unsecured and cover the main HTTP methods (Get, Post, Put, Delete)
 - Setup async quarkus-rest-client-mutiny based client pointing to our endpoints.

### Impact on testsuites and testing automation:
 - Ensure this coverage works both in JVM and NATIVE mode
 - Integration tests covered by beefy-scenarios testsuite

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features

## Automated test development

- [Quarkus/Mutiny Http Client](https://github.com/quarkus-qe/beefy-scenarios/tree/master/013-quarkus-oidc-restclient): Ping API will invoke Pong API using Quarkus Mutiny RestClient and returns the expected "ping pong" output.

## Advanced topics for test development
The following advanced topics are listed for future consideration. 
 - Server sent events, Soap, or other protocols over Quarkus Mutiny/client.