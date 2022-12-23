# QUARKUS-2743 - Consuming multiple media types from a resource method does not work

JIRA: https://issues.redhat.com/browse/QUARKUS-2743

Quarkus documentation: https://quarkus.io/guides/resteasy-reactive#negotiation

PR: https://github.com/quarkusio/quarkus/pull/29734

Upstream issue: https://github.com/quarkusio/quarkus/issues/29732

Consuming multiple media types from a resource method works in Quarkus 2.13.6.

## Scope of testing

Tests should verify:

- Response must contain 415 Unsupported Media Type when:
  - no resource method consumes request standard content type nor wild card
  - no resource method consumes request custom content type nor wild card
- Matching request to resource method works when multiple endpoints listen on the same path and:
  - resource method consumes single media type
  - resource method consumes one of media types
  - the most specific reference is selected when selecting from wildcards and media content subtypes
  - resource method is not annotated with `@Consumes`
  - request contains accept headers; the headers must serve as additional selection parameter when consumed content types are matching
  - resource methods for both exact match and wildcard exists; exact match must have priority over wildcard
  - request contains media subtype wildcard
  - request contains quality values; quality values must determine selected content type

### Existing tests
Upstream test coverage verifies simple scenario with multiple content types. 
Quarkus QE Test Suite does not cover multiple media types matching.

## Impact on test suites and test environment

New scenarios are going to be added to the `http/JAXRS-reactive` module of Quarkus Test suite. Multiple test method are
going to be needed as well as respective resource class with methods consuming and producing huge variety of content types.
All verifications can be with HTTP response status and body checks.
Tests only needs to be run on bare metal instances as OpenShift run would not provide extra information.

### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be below 4 minutes
- No measurable additional requirements for a Native image scenario

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Matching Requests to Resource Methods](https://jakarta.ee/specifications/restful-ws/3.1/jakarta-restful-ws-spec-3.1.html#mapping_requests_to_java_methods)
- [Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [Quality Values](https://www.rfc-editor.org/rfc/rfc9110.html#section-12.4.2)
- [Content Negotiation Fields](https://www.rfc-editor.org/rfc/rfc9110.html#section-12.5)