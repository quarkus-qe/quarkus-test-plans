QUARKUS-3660 Stop productizing quarkus-smallrye-opentracing
## Links to the resources
Quarkus Jira: https://issues.redhat.com/browse/QUARKUS-3660
QE Jira: https://issues.redhat.com/browse/QQE-574


## Scope of the testing
Smallrye-opentracing is not supported anymore, we need to drop all checks for it.


## Automated test development
- Remove quarkus-smallrye-opentracing from marete config for 3.8 (already done)
- Remove quarkus-smallrye-opentracing from 3.8 branch of startstop (was never added)

## Advanced topics for test development
 - Remove `monitoring-microprofile-opentracing` module from the main TS
 - Replace opentracing with opentelemetry in monitoring-opentracing-reactive-grpc in the main TS and rename the module
