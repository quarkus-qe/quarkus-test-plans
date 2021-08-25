# QUARKUS-1075 Overall improvements around RESTEasy Reactive

JIRA link: https://issues.redhat.com/browse/QUARKUS-1075

Test plan covers RESTEasy Reactive functionality provided by `quarkus-resteasy-reactive` and `quarkus-rest-client-reactive` extensions,
with emphasis on multipart form data support.

https://quarkus.io/version/main/guides/resteasy-reactive

https://quarkus.io/version/main/guides/rest-client-reactive

## Scope of the testing
- Endpoints using basic HTTP methods: GET, POST, PUT, DELETE.
- Endpoints with multipart data payload.
- Execution model of RESTEasy Reactive.
- REST Client Reactive
- Traceable resources using `quarkus-smallrye-opentracing`.

### Impact on test suites and testing automation:
There are ~3 new modules planned for QuarkusTS. Estimated increase in run time:
- bare metal
    - 5 minutes for JVM mode
    - 20 minutes for native mode
- OpenShift
    - 10 minutes for JVM mode
    - 25 minutes for native mode

3 new modules in Quickstarts acceptance jobs: ~5 minutes increase.

## Automated test development
- Identify existing coverage in Quarkus integration tests.
- Adopt existing RESTEasy scenarios for RESTEasy Reactive:
    - Transfer existing RESTEasy-related scenarios from BeefyTS and OpenShiftTS into QuarkusTS (https://github.com/quarkus-qe/quarkus-test-suite).
        - https://github.com/quarkus-qe/beefy-scenarios/tree/main/001-quarkus-getting-started-with-jaxrs
        - https://github.com/quarkus-qe/quarkus-openshift-test-suite/tree/main/http
    - Add reactive equivalents to the transferred scenarios in QuarkusTS.
    - Add test case covering execution model (blocking / non-blocking) of RESTEasy Reactive (https://quarkus.io/guides/resteasy-reactive#execution-model-blocking-non-blocking) to a suitable reactive scenario.
- Add relevant quickstarts to the acceptance set (https://gitlab.cee.redhat.com/quarkus-qe/jenkins-jobs/-/blob/main/jobs/rhbq/rhel8_jdk11_quickstarts_ts_jvm.groovy):
    - `getting-started-reactive`
    - `getting-started-reactive-crud`
    - `rest-client-reactive-quickstart`

## Advanced topics for test development
- RESTEasy Reactive in combination with Hibernate Reactive (`quarkus-hibernate-reactive`)
    - Hibernate Reactive is covered in `QUARKUS-1079`.
- RESTEasy Reactive in combination with Kafka Reactive Messaging (`quarkus-smallrye-reactive-messaging-kafka`)
    - Kafka Reactive Messaging is covered in `QUARKUS-1083`.
- Performance comparison between classical RESTEasy and RESTEasy Reactive - manual monitoring of heap memory and CPU under load.
- Data serialization - JSON (`quarkus-resteasy-reactive-jackson` / `quarkus-resteasy-reactive-jsonb`), XML, other.
- HTTP Caching.

## Time estimate
5-10 MD basic plan execution, +5 MD advanced topics.
