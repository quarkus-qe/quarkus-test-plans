# QUARKUS-1490 Use BuildItem to enable use of TestContainers shared network

JIRA link: https://issues.redhat.com/browse/QUARKUS-1490

Use BuildItem to enable use of TestContainers shared network across the various dev-services.

## Scope of the testing
Get familiar with the feature implemented in https://github.com/quarkusio/quarkus/pull/21863 
Check the test coverage and availability of documentation (if needed). 

## Impact on test suites and testing automation
No new test development is needed. Coverage in upstream test-suites is sufficient.

`DevServicesSharedNetworkBuildItem` is used in all the dev-services modules (see the PR #21863) and is therefore exercised in the various tests that use dev services.
Furthermore, tests like container-build-jib-with-postgresql are the ones that actually test this.

On `kiegroup` there is [tracing-decision extension](https://github.com/kiegroup/kogito-runtimes/blob/main/quarkus/addons/tracing-decision/)
that re-uses DevServices support for PostgreSQL and builds `DevServicesSharedNetworkBuildItem` that then allows us to use DevServices PostgreSQL instance.
For details see `DevServicesSetupEnvironmentProcessor`and related BuildItem classes in the package.

## Automated test development
- N/A
