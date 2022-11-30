# QUARKUS-2246 - Integrate Vertx HTTP extension with custom CredentialsProvider

JIRA: https://issues.redhat.com/browse/QUARKUS-2246

Integrate Vertx HTTP extension with custom CredentialsProvider, main motivation is that RHPAM product team can use FileVaultCredentialsProvider to support their use cases.

RHBQ product team provides the means to use custom CredentialsProvider.

## Scope of the testing

- Review the PR introducing the functionality
- Evaluate the test coverage available in upstream
- Check if additional test coverage is needed
- Implement the additional test coverage

The primary target should be `integration-tests` module of the upstream Quarkus repo.

### Impact on resources
- No noticeable impact on resources needed for TS executions
