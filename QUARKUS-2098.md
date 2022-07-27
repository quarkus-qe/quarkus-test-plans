# QUARKUS-2098 - Fix support of media types with suffixes

JIRA link: https://issues.redhat.com/browse/QUARKUS-2098

Test plan covers RESTEasy Reactive functionality provided by `quarkus-resteasy-reactive` and `quarkus-rest-client-reactive` extensions,
with emphasis on multipart form data support.

https://quarkus.io/version/main/guides/resteasy-reactive

https://quarkus.io/version/main/guides/rest-client-reactive

## Scope of the testing
- Endpoints with mediatypes with suffix(e.g. application/test+json).

### Impact on test suites and testing automation:
Negligible. We will add several tests and corresponding endpoints.

## Automated test development
- Add test for mediatype with subtype and suffix
- Add tests for mediatype with either subtype or suffix
- Tests to make sure, that priority is
  1. subtype+suffix
  2. only subtype
  3. only suffix 

## Time estimate
3 developer days
