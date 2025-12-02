# QUARKUS-6683 - Provide a fluent API for CSRF programmatic set up

JIRA: https://issues.redhat.com/browse/QUARKUS-6683

Upstream PR: https://github.com/quarkusio/quarkus/pull/49618

Support programmatic set up for CSRF form protection using fluent API

Support scope: Development preview - https://access.redhat.com/support/offerings/devpreview

Upstream documentation: https://quarkus.io/guides/security-csrf-prevention#csrf-prevention-programmatic-set-up

## Scope of testing
Verify programmatic setup of CSRF form protection.
Cover this functionality with new tests in QE TS.
Test will require rendering of HTML forms, which are protected by CSRF tokens.
Form can be parsed and submitted in a similar way like REST API, so no browser will be required.

Goal of this testing is to smoke test that programmatic configuration works and indeed is actually takes effect.
It is not intended as extensive testing of all config options, as those are handled in upstream Quarkus tests.

New tests will be added into module `security/form-authn`.
CSRF configuration will need to be disabled by default in the module, to not interfere with other tests.

### Test cases
Test that CSRF, when configured via programmatic API, can be customized for CSRF form-field-name and cookie-name.
Verify that these are set correctly in generated HTML form as well as accepted in submitted form.

Verify that submitting form with wrong token will fail.
This is not strictly related to the tested feature, but will verify that CSRF protection in indeed active, 
thus eliminating false positives in tests.

## Existing test coverage
Upstream TS has test coverage for most configuration options, but no integration tests.
QE TS has no coverage for CSRF protection yet.

## Impact on the test suite
Extensions `quarkus-security` and `quarkus-rest-csrf` as well as test class and corresponding sources will be added into module `security/form-authn`.

## Impact of resources
A new test classes will be added expecting additional execution time of few minutes for JVM and native for both baremetal and OCP.

## Contacts
* Tester: Martin Ocenas <mocenas@ibm.com>
