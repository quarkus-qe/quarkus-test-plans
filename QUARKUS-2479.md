# QUARKUS-2479 SmallRye Config SecretKeys support

Jira: https://issues.redhat.com/browse/QUARKUS-2479

This test plan covers support of secret keys in SmallRye config

## Scope of the testing
Following topics should be covered:

1. Verify, that some config properties can be marked as secret.
2. Verify, that usage of these secrets leads to an exception.
3. Verify, that these secrets cannot be used when injected and when accessed directly.
4. Verify, that these secrets can be used only within doUnlocked section.
5. Coverage should exist for both JVM and native mode in baremetal.

### Impact on test suites and testing automation
- One new test class added for Kafka Dev Services
- One test method in sql/hibernate reactive refactored 

### Impact on resources:
- No additional requirements for resources in lab
- Required additional time for the test execution in JVM mode should be less than a minute.
- Since current implementation of config do not have baremetal tests, we have to add a separate test class, so duration of another native compilation will be added to total run time of the TS.


## Advanced topics for test development
- No advanced topics found

## References

- [feature description](https://smallrye.io/smallrye-config/2.11.1/config/secret-keys/)
