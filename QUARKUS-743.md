# QUARKUS-743 - Verify Code Starters

JIRA link: https://issues.redhat.com/browse/QUARKUS-743

The goal is to verify the code starters available in some Quarkus extensions.

For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing

- For Quarkus 1.11, the Quarkus extensions that provide a code starter are:

```
quarkus-amazon-lambda
quarkus-azure-functions-http
quarkus-config-yaml
quarkus-funqy-amazon-lambda
quarkus-funqy-http
quarkus-funqy-knative-events
quarkus-google-cloud-functions
quarkus-google-cloud-functions-http
quarkus-logging-json
quarkus-picocli
quarkus-qute
quarkus-resteasy
quarkus-resteasy-jackson
quarkus-resteasy-reactive
quarkus-spring-web
```

The list has been provided by running: `curl https://code.quarkus.io/api/extensions 2>/dev/null | jq '.[] | select(.tags[] | contains("provides-example")) | .id' | sort | uniq`.

For each supported and tech preview Quarkus extension with code starter:
 - Verify that the Quarkus extensions defined in the `pom.xml` are marked with the `provides-example` tag in `code.quarkus.io`. 
 - Verify that the code start compiles and runs in DEV/JAR modes.

Following actions were taken to ensure test coverage and automation for native image:
 - Define the scope of the testing
 - Analyze the possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden

 As part of the development, code starters are already being verified using `Maven` in [these tests](https://github.com/quarkusio/quarkus/blob/main/integration-tests/devtools/src/test/java/io/quarkus/devtools/codestarts/quarkus/QuarkusCodestartRunIT.java#L74-L90). 

### Impact on testsuites and testing automation:
 - Test development in Start-Stop testsuite in order to cover:
   - Verify that the code start compiles and runs in DEV/JAR modes.
 - Ensure this coverage works both in JVM and NATIVE mode

### Impact on resources:
 - QUARKUS-743 covers both JVM and NATIVE mode, Native image imposes multiple times prolonged execution time and increased memory requirements for the build

Our current testing capacity is sufficient to cover this testing.

If multiple streams get introduced we need to have increased capacity in the lab.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - Ensure platform specific features or components have appropriate test coverage

## Advanced topics for test development
The following advanced topics are listed for future consideration. 
 - Coverage of UI functionality
