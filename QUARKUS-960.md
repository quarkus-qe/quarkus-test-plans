# Quarkus-960 - Improved getting started experience with CLI
JIRA link: https://issues.redhat.com/browse/QUARKUS-960

There was an existing CLI tool in Quarkus that has been fully reworked as part of QUARKUS-960. 

So, the goal is to verify proper functionality of the new Quarkus CLI tool.

## Scope of the testing

The Quarkus CLI is compatible with Maven, Gradle and Jbang tools, however RHBQ is only focused on Maven, so Gradle and Jbang will be covered as part of the test coverage in upstream.

Test development will focus on the supported features scoped for RHBQ 2.2:
- Create applications (with a set of extensions)
- List extensions
- Build existing Quarkus applications on JVM
- Build existing Quarkus applications on Native (also using container configuration and builder images)
- Run Quarkus applications on DEV mode
- Support the usage of multiple BOMs
- Add tests for special characters in names and paths
- Ensure that there is a `help` option for every command and subcommand
- Ensure that `quarkus completion bash` generates the expected files to enable autocompletion.

### Impact on testsuites and testing automation:
 - Enhance Quarkus QE test framework to cover all the features that are part of the scope
 - Add test coverage into the Quarkus QE test suite only for baremetal
 - Ensure this coverage works both in JVM and NATIVE mode 

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use

## Manual verification
 - Ensure that there is CI Delivery mechanism to release Quarkus CLI tool when releasing Quarkus
 - Ensure Quarkus CLI is documented
 - Check command autocompletion (also for subcommands)
 - Ensure Quarkus CLI works fine on Windows (Windows compability will be automatically covered in the future)
 - Compare functionality and user experience of the Quarkus CLI to other command tools such as [Spring Boot CLI](https://docs.spring.io/spring-boot/docs/current/reference/html/cli.html) and [Micronout CLI](https://docs.micronaut.io/latest/guide/index.html#buildCLI)
