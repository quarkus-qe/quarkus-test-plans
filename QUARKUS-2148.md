# QUARKUS-2148 - Add support for sending query params as using a Map in Reactive Rest Client

JIRA link: https://issues.redhat.com/browse/QUARKUS-2148

PR: https://github.com/quarkusio/quarkus/pull/24783

Upstream issue: https://github.com/quarkusio/quarkus/issues/24764


The REST Client Reactive now supports passing query parameters as map. This can be very useful if query parameters are not known
in advance. For primitive data types and Java classes that are not a `Map` implementation, `@RestQuery` annotation still resolves 
query key from `@RestQuery#value()`, or from a formal parameter name. Each `Map.Entry` represent a key and value of a query parameter.
Query parameters whose values are arrays can be passed as a `MultivaluedMap`;

## Scope of the testing

Tests should verify:
- User is able to pass query parameters as map.
- Primitive and complex data types and arrays can be sent.
- Multiple query parameter maps can be specified.
- Query parameter map works in a native mode.

### Impact on testsuites and testing automation:

New scenario is going to be added to the `http/rest-client-reactive` module of Quarkus Test suite. One test method is
going to be needed as well as respective endpoint that handles dynamic number of query parameters. All verifications can be done
with one call as `@RestQuery` is repeatable.

### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be below 4 seconds
- No measurable additional requirements for a Native image scenario

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Advanced topics for test development

- No advanced topics found

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [USING THE REST CLIENT REACTIVE](https://quarkus.io/guides/rest-client-reactive)
- [Accessing request parameters](https://quarkus.io/guides/resteasy-reactive#accessing-request-parameters)
- [URI query parameters](https://datatracker.ietf.org/doc/html/rfc3986#section-3.4)
