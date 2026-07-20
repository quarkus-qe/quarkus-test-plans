# QUARKUS-7309 - Support quarkus-http-problem extension (RFC 9457 Problem Details) in RHBQ

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7309

Upstream extension: https://github.com/quarkiverse/quarkus-http-problem

The `quarkus-http-problem` extension implements RFC 9457 (Problem Details for HTTP APIs).
It maps Java exceptions to standardized `application/problem+json` responses, preventing stack trace leaks and providing consistent error handling.
The extension is in Quarkiverse and will be productized as tech preview for both RHBQ and IBQ 3.40.
Platform BOM onboarding is not yet completed but is in progress and targeted for 3.39.
No documentation exists beyond the README .
The upstream LTS branch and backporting policies have not been defined yet.

## Scope of the testing

Verify that the extension correctly produces RFC 9457-compliant `application/problem+json` responses with both Quarkus REST and RESTEasy Classic.

Tests will cover:

* `HttpProblem` with standard fields (`type`, `title`, `status`, `detail`, `instance`) produces a compliant response with `application/problem+json` content type.
* The `instance` field preserves path separators as a valid URI reference.
* Custom fields and custom HTTP headers added via `HttpProblem.builder()` appear in the response.
* Built-in exception mappers produce the expected status codes: `NotFoundException` (404), `ForbiddenException` (403), `UnauthorizedException` (401), `WebApplicationException` (custom status).
* `ConstraintViolationException` produces HTTP 400 with a `violations` array, and status is configurable to 422.
* A custom `ProblemPostProcessor` CDI bean modifies the problem before serialization.
* MDC property inclusion via `quarkus.http-problem.include-mdc-properties` adds configured properties to the response.
* REST client `ThrowingHttpProblemClientExceptionMapper` deserializes `application/problem+json` from an upstream service.
* Disabling a built-in mapper via `quarkus.http-problem.mapper.<name>.enabled=false` prevents it from handling the exception.
* The generated OpenAPI specification includes the `HttpProblem` schema.
* An unhandled exception produces HTTP 500 with no stack trace or internal class names leaked.
* Jackson and JSON-B serialization both produce correct output.

## Existing test coverage

Upstream tests: https://github.com/quarkiverse/quarkus-http-problem/tree/main/integration-test

Upstream coverage includes integration tests for core mapping, security mappers, validation, MDC, disabled mappers, REST client, and OpenAPI, all with native mode counterparts.
Tests use Quarkus dev services.

The QE test suite does not contain any tests for this extension.

### Impact on test suites and testing automation

* A new `http/http-problem` module will be created with Quarkus REST and Jackson as the primary configuration.
* RESTEasy Classic coverage will be verified using a separate test application within the same module.
* JSON-B compatibility will be verified using a separate test application within the same module.
* The extension will be built from the upstream `main` branch and added to the custom platform build for daily runs.

#### Acceptance jobs
* Marete configs will be updated for `io.quarkiverse.httpproblem:quarkus-http-problem`
* Start-stop tests to verify the extension starts and stops without errors
* Verify correct support level on code.quarkus and platform registry

### Impact on resources

The new module is a lightweight Quarkus application with no external service dependencies, so the impact should be minimal.
Tests will be executed on baremetal and OpenShift in JVM and native mode.
The estimated execution time increases by a few minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Reviewed the extension repository, recent pull requests, and known issues
- Reviewed RFC 9457 (Problem Details for HTTP APIs)
- Focus on exploratory testing of the feature

## Contacts

* Tester: Slimane Abzar <sabzar@redhat.com>

## References

- https://github.com/quarkiverse/quarkus-http-problem
- https://datatracker.ietf.org/doc/html/rfc9457
