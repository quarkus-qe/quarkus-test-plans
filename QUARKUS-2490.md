# QUARKUS-2490 Add HTTP headers for specific paths

JIRA link: https://issues.redhat.com/browse/QUARKUS-2490

Quarkus documentation: https://quarkus.io/guides/http-reference#additional-http-headers-per-path

Quarkus HTTP layer (provided by built-in Eclipse Vert.x) enables users to add custom HTTP response headers,
either globally, or for a particular resource path and HTTP method. The limitation of this solution is
the inability to specify a custom header for multiple paths at once. This shortcoming is being addressed by
this new functionality.

The users can now add custom response headers for a group of paths defined by a regular expression, and also limit
the HTTP methods which this definition applies to. There is also a priority evaluation mechanism for cases when multiple
rules apply to the same resource paths.

## Scope of testing

### Existing tests
The custom HTTP headers are being tested in
[Quarkus upstream integration tests](https://github.com/quarkusio/quarkus/tree/main/integration-tests/vertx-http).
The coverage includes:
- Global header definition (for all paths).
- Single path header definition + HTTP methods definition.
- Single path header override (defined in `application.properties` and overriden by endpoint `Response`).
- Multiple paths header definition + HTTP methods definition.
- Multiple paths header override.
- Multiple paths order.

This covers the whole custom header functionality.

However, the upstream tests do not consider execution in native mode and in the OpenShift environment.

## Automated test development
Adopt the upstream integration tests (see above) into the [Quarkus QE test suite](https://github.com/quarkus-qe/quarkus-test-suite)
and complement them with an OpenShift scenario.
Suitable modules:
- `http/http-advanced`.
- `http/http-advanced-reactive` - to make sure both execution modes behave accordingly.

Note: all `http/*` modules in QE TS get executed in native mode in both baremetal and OpenShift environment.

## Impact on test suites and test environment
2 new scenarios (in 2 modules), each one will increase the execution time by circa:
2 / 5 / 4 / 8 minutes in JVM / native / OpenShift JVM / OpenShift native mode.

No other impacts are expected.
