# QUARKUS-8370 - Allow the system message to depend on the model in use

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8370

Upstream PR: https://github.com/quarkiverse/quarkus-langchain4j/pull/2563

This PR introduces new system message provider that receives the InvocationContext and can inspect the model serving each call, so the system message can be chosen per model provider.

## Scope of the testing

Add a new SystemMessageProvider. It should change system message depending on model provider in use. Since we only test openai at the moment, the tests should check, that openai-specific message is used.   

## Existing test coverage

Upstream tests cover the scenario above but only on unit test level. 

### Impact on test suites and testing automation

* Ideally the changes and tests should be implemented upstream, in OpenAI samples folder
* If upstream team opposes that,the changes and tests for them should be put into our TS, inside `ai/langchain4j` module

### Impact on resources

The added tests should have minimal impact on resources:

- There will be either 0 or one new test method
- Tests will be executed on baremetal only

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documententation on rest client.

## Contacts

* Tester: Fedor Dudinskii fdudinsk@ibm.com

## References
* https://docs.quarkiverse.io/quarkus-langchain4j/dev/ai-services.html#_systemmessage