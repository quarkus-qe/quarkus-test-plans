# QUARKUS-2991 Support for MicroProfile 6

JIRA link: https://issues.redhat.com/browse/QUARKUS-2991

Quarkus documentation:
- https://quarkus.io/version/main/guides/opentelemetry
- https://quarkus.io/version/main/guides/openapi-swaggerui
- https://quarkus.io/version/main/guides/security-jwt
- https://quarkus.io/version/main/guides/smallrye-metrics

MicroProfile documentation:
- https://microprofile.io/2023/01/10/microprofile-6-0-release
- https://microprofile.io/2021/12/07/microprofile-5-0-release


## Scope of testing

MicroProfile 6.0 brings upgrades a few of its standards.

RHBQ 3.x aims to support the following set of MicroProfile specifications:
- Telemetry 1.0 - new in MP 6
- Open API 3.1 - updated in MP 6
- Rest Client 3.0
- Config 3.0
- Fault Tolerance 4.0
- JWT Authentication  2.1 - updated in MP 6
- Health 4.0
- Jakarta EE 10 Core Profile - updated in MP 6

RHBQ 3.x should also support:
- Reactive Messaging 3.0 - updated in MP 6
- Reactive Streams Operators 3.0 - updated in MP 6
- Context Propagation 1.3
- Open Tracing 3.0
- GraphQL  2.0

Support for the specifications means compliance with respective Technology Compatibility Kits (TCKs) and/or additional tests.
Each standard should have sufficient test coverage which addresses key features.

### Existing tests

Following table summarizes numbers of tests being executed in community Quarkus repository for each MP specification:

| Test type                   | Quarkus module                        |  Quarkus 2.13.7.Final |  Quarkus main |
|-----------------------------|---------------------------------------|----------------------:|--------------:|
| TCKs                        | microprofile-config                   |                   372 |           372 |
|                             | microprofile-context-propagation      |                    82 |            82 |
|                             | microprofile-fault-tolerance          |                   433 |           435 |
|                             | microprofile-graphql                  |                   236 |           236 |
|                             | microprofile-health                   |                    28 |            28 |
|                             | microprofile-jwt                      |                   190 |           200 |
|                             | microprofile-lra                      |                   133 |           133 |
|                             | microprofile-metrics                  |                   233 |           233 |
|                             | microprofile-openapi                  |                   244 |           317 |
|                             | microprofile-opentelemetry            |                   N/A |             1 |
|                             | microprofile-opentracing              |                    66 |            66 |
|                             | microprofile-reactive-messaging       |                    57 |            57 |
|                             | microprofile-rest-client              |   191<br>(6 failures) |           191 |
|                             | microprofile-rest-client-reactive     |                   155 |           139 |
|                             |                                       |                       |               |
| TCKs - missing              | Jakarta EE 10 Core Profile            |                       |               |
|                             | Reactive Streams Operators 3.0        |                       |               |
|                             |                                       |                       |               |
| Integration tests           | grpc-health                           |                     2 |             2 |
|                             | narayana-lra                          |                     2 |             2 |
|                             | opentelemetry                         |                    13 |            15 |
|                             | opentelemetry-grpc                    |                     1 |             1 |
|                             | opentelemetry-jdbc-instrumentation    |                   N/A |             3 |
|                             | opentelemetry-reactive                |      6<br>(1 skipped) |             6 |
|                             | opentelemetry-reactive-messaging      |                   N/A |             2 |
|                             | opentelemetry-spi                     |                   N/A |             3 |
|                             | opentelemetry-vertx                   |                     5 |             5 |
|                             | reactive-messaging-amqp               |                     1 |             1 |
|                             | reactive-messaging-hibernate-orm      |                     1 |             2 |
|                             | reactive-messaging-hibernate-reactive |                     1 |             2 |
|                             | reactive-messaging-kafka              |                     3 |             4 |
|                             | reactive-messaging-mqtt               |                   N/A |             1 |
|                             | reactive-messaging-rabbitmq           |                     1 |             1 |
|                             | reactive-messaging-rabbitmq-dyn       |                     1 |             1 |
|                             | smallrye-config                       |                    25 |            29 |
|                             | smallrye-context-propagation          |     30<br>(1 skipped) |            28 |
|                             | smallrye-graphql                      |                     5 |             5 |
|                             | smallrye-graphql-client               |                     9 |            10 |
|                             | smallrye-jwt-oidc-webapp              |                     6 |             6 |
|                             | smallrye-jwt-token-propagation        |                     9 |             9 |
|                             | smallrye-metrics                      |                    20 |            20 |
|                             | smallrye-opentracing                  |                    11 |            11 |
|                             |                                       |                       |               |
| Integration tests - missing | Open API 3.1                          |                       |               |
|                             | Fault Tolerance 4.0                   |                       |               |
|                             | Jakarta EE 10 Core Profile            |                       |               |
|                             | Reactive Streams Operators 3.0        |                       |               |

### Identified problematic areas

#### Priority

Telemetry 1.0
- Currently, the TCK is inactive in Quarkus `main` due to an API incompatibility in the latest MP release.
  An update of the TCK in MP repository is beyond Quarkus' scope.
- AIs:
  - Asses the scope of the test coverage in the Quarkus integration tests.
  - Asses the existing tests in QE TS.
  - Develop new integration test scenario in QE TS.

#### Deferred

Rest Client 3.0
- No updates.
- AI: Check the decline in number of tests in `microprofile-rest-client-reactive` TCK.

Reactive Messaging 3.0
- New updates, but TCK is unchanged.
- AI: TD for changes not covered in the TCKs and Quarkus integration tests.

Context Propagation 1.3
- Updated from 1.2 since Quarkus 2.13.
- 1.3 does not introduce any changes apart from introduction of Jakarta namespace and corresponding update of the TCKs.
- AI: Check the decline in number of tests in `smallrye-context-propagation` module of integration tests.

Jakarta EE 10 Core Profile
- New updates, no tests found.
- AI: TCK or TD is needed.

Reactive Streams Operators 3.0
- New updates, no tests found.
- AI: TCK or TD is needed.

Open Tracing 3.0
- Replaced by Telemetry 1.0.
- No TD needed.

GraphQL 2.0
- No functional changes in MP 5/6
- Existing TCKs, Quarkus integration tests and QE TS integration tests provide adequate coverage.
- No TD needed.

## Automated test development

Integration scenario in QE TS: `opentelemetry` + `smallrye-openapi` + `smallrye-jwt`.
- New module `monitoring/opentelemetry-openapi-jwt`