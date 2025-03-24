# QUARKUS-5654 Update MP REST Client to 4.0

JIRA: https://issues.redhat.com/browse/QUARKUS-5654

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/43959

## Scope of the testing
- New overloaded `baseUri(String uri)` method so users don’t have to call `URI.create()`
- New `header(String name, Object value)` method for adding headers to Client instances
- Clarify specification CDI support (`@RestClient` qualifier is not optional)
- Add multi-part processing example to specification.
- Clarify the specification that when a client injected as a CDI bean falls out of scope, the client is closed

## Getting familiar with the feature
Getting familiar with
- https://download.eclipse.org/microprofile/microprofile-rest-client-4.0/microprofile-rest-client-spec-4.0.html

## Existing test coverage
* New overloaded `baseUri(String uri)` method so users don’t have to call URI.create() is covered upstream.
* New `header(String name, Object value)` method for adding headers to Client instances is covered upstream.

### Impact on test suites and testing automation
None. Both new features are already covered in upstream tests. Examples and clarified tests doesn't affect Quarkus (and CDI support is handled this way in Quarkus since 2.13 at least).
 
### Impact on resources
None

## Contacts
* Tester: Fedor Dudinskii <fdudinsk@redhat.com>

## References
- https://download.eclipse.org/microprofile/microprofile-rest-client-4.0/microprofile-rest-client-spec-4.0.html
- https://quarkus.io/guides/rest-client
- https://quarkus.io/version/2.13/guides/rest-client#create-the-jax-rs-resource