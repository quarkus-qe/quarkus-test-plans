# QUARKUS-2485 SmallRye GraphQL non-blocking support

JIRA link: https://issues.redhat.com/browse/QUARKUS-2485

Quarkus documentation: https://quarkus.io/version/main/guides/smallrye-graphql

Related topic: [QUARKUS-1398 GraphQL product support](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-1398.md).

Existing GraphQL functionality provided by the `smallrye-graphql` (and `smallrye-graphql-client`) extension
was limited in terms of execution model: the extension used blocking execution only.

With this new addition, support for non-blocking execution has been introduced. The appropriate execution model
selection is based on return types of GraphQL endpoint and can be altered by `@Blocking`/`@NonBlocking` annotations.
This approach corresponds with the behavior of RESTEasy Classic / RESTEasy Reactive
(see also https://github.com/quarkusio/quarkus/pull/25194).

## Scope of testing

### Existing tests
An extensive test coverage already exists in several places:
- [Quarkus upstream unit tests](https://github.com/quarkusio/quarkus/tree/main/extensions/smallrye-graphql/deployment/src/test)
- [Quarkus upstream integration tests](https://github.com/quarkusio/quarkus/tree/main/integration-tests/smallrye-graphql)
- [Quarkus quickstarts](https://github.com/quarkusio/quarkus-quickstarts/tree/main/microprofile-graphql-quickstart)
- Quarkus QE test suite
  - [http/graphql](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/http/graphql)
  - [http/graphql-telemetry](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/http/graphql-telemetry)

Key functionality which is addressed in the existing tests:
- POST Query
- POST Query reactive
- GET Query (GraphQL over HTTP)
- @Source + Batching
- Interfaces
- Unions
- Mutation
- Subscription (WebSocket integration)
- @DefaultValue
- Context injection
- @AdaptWith
- Dev UI
- OpenTracing and MicroProfile Metrics
- OpenTelemetry and Micrometer

Test coverage is missing for:
- GET Query (GraphQL over HTTP) reactive
- Mutation reactive
- @DefaultValue reactive
- Context injection reactive
- @AdaptToScalar
- Maps support
- @ErrorCode

## Automated test development
The missing features can be covered by adding test in the existing `http/graphql` module in the Quarkus QE test suite:
- 1 new scenario for GET queries (requires distinct application configuration).
- Other features may be added to existing scenarios.

## Impact on test suites and test environment
- Extending existing scenarios: minimal impact.
- 1 new scenario: impact on execution time.