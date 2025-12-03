# QUARKUS-6684 Provide a fluent API for CORS programmatic set up

JIRA: https://issues.redhat.com/browse/QUARKUS-6684

Upstream PR: https://github.com/quarkusio/quarkus/pull/49400 

Support programmatic set up for Cross-Origin Resource Sharing (CORS) using fluent API

Support scope: Developer preview

Upstream documentation: https://quarkus.io/guides/security-cors#cors-filter-programmatic-set-up

## Scope of testing
Verify programmatic set up of CORS protection.
Cover this functionality with new tests in QE TS.
CORS in Quarkus does two things, tests will cover both:
* Setup HTTP header "access-control-allow-origin" in normal HTTP responses (not in access denied etc.).
* Verify that incoming request have correct "Origin" HTTP header and deny access if not.

Programmatic CORS API provide two ways for configuration (see [docs](https://quarkus.io/guides/security-cors#cors-filter-programmatic-set-up)), tests will cover both:
* Simple - specify only allowed origin and validate against it.
* Full - Using `CORS.builder()` allows full CORS configuration.

Goal of these tests is to verify that CORS setup doesn't cause problems and is actually effective.
Tests will not be extensive, they will not cover all configuration options, as these tests are already done in upstream Quarkus.

### Test cases
Tests will cover both Simple and Full (see above) programmatic API for CORS setup.
For those will verify basic scenarios - access granted and denied.
For full configuration will verify combination of origin and HTTP method.
Also, will verify that HTTP header "access-control-allow-origin" is set correctly.

## Existing test coverage
QE TS does not contain tests for CORS functionality in Quarkus yet.
CORS configuration is present in several keycloak-related modules to help those work flawlessly, 
but nothing is targeted at testing the CORS itself in quarkus.

## Impact on test suite
New test classes will be added into module `security/form-authn`.

## Impact on resources
A new test classes will be added expecting additional execution time of few minutes on JVM for baremetal and OCP.
Since testing both Simple and Full CORS config will require two apps to build, 
native builds for baremetal will take 3 minutes and 7 minutes for OCP.

## Contacts
* Tester: Martin Ocenas <mocenas@ibm.com>

## References
https://quarkus.io/guides/security-cors#cors-filter-programmatic-set-up
