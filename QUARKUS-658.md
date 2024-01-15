# QUARKUS-658 - Reactive REST client - support HTTP/2

JIRA: https://issues.redhat.com/browse/QUARKUS-658

PR: https://github.com/quarkusio/quarkus/pull/31192

Upstream issue: https://github.com/quarkusio/quarkus/issues/13969

Quarkus should be able to use http/2 on client side to be able to comunicate with other microservices.

## Scope of the testing
The scope of the tests will cover the usage of reactive rest client inside Quarkus. The complex scenarios like sending requests to different APIs/microservices won't be covered.

Scope in point will be:
- Run Quarkus application and send requests using `http/2` to verify that server response with `http/2`. 
- Run Quarkus application with enabled Application-Layer Protocol Negotiation (alpn) to verify that correct http version is used.
- Check the usecases using HTTP and HTTPS.

These points will be covered for a single REST client and all REST clients to verify these properties:

```
// All REST clients
quarkus.rest-client.http2=true
quarkus.rest-client.alpn=true

// Single REST Client:
quarkus.rest-client.extensions-api.http2=true
quarkus.rest-client.extensions-api.alpn=true
```

New tests will be run in jvm and native mode.

## Existing test coverage
- We already have tests for http/2 client side to test client sync and async. These tests are both in reactive and non-reactive modules and are disabled.
	- Non-reactive tests will be removed as only reactive client should be used.
	- Reactive tests need to be updated as they are failing.

### Impact on test suites and testing automation
- New tests will be added inside [http/rest-client-reactive](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/http/rest-client-reactive) module.

### Impact on resources
- There will be longer tests execution for jvm and native runs of QE TS.

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>

## References
- [PR quarkusio/quarkus#31192](https://github.com/quarkusio/quarkus/pull/31192)
- [Issue quarkusio/quarkus#13969](https://github.com/quarkusio/quarkus/issues/13969)
