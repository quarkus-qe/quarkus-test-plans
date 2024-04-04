QUARKUS-4144 Stop productizing smallrye-metrics

## Links to the resources
Quarkus Jira: https://issues.redhat.com/browse/QUARKUS-4144
QE Jira: https://issues.redhat.com/browse/QQE-573


## Scope of the testing
Smallrye-metrics is not supported anymore, we need to drop all checks for it.


## Automated test development
- Remove quarkus-smallrye-metrics from marete config for 3.8
- Remove quarkus-smallrye-metrics from 3.8 and main branches of startstop

## Advanced topics for test development
 - Remove `smallrye-metrics` from javaee, super-size and build-time analytics modules of the main TS
