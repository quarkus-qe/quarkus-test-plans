# QUARKUS-7931 - Introduce `ModelBuilderCustomizer` interface for model builder customization

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7931

Upstream PR: https://github.com/quarkiverse/quarkus-langchain4j/pull/2366

This PR introduces new interface, that allows customisation of model builder. The intended purpose is to allow users to get access to new features of models, even if those features are not (yet) supported in Langchain4j.

## Existing test coverage

Upstream has extensive unit tests, but no integration or end-to-end tests. 

## Scope of the testing

No need to test all possible customisations, this is covered in upstream. We need to check, if this really works:
- with current OpenAI API
- with streaming models (reactive endpoints)
- in native mode. 

### Impact on test suites and testing automation

- New tests will be added into `ai/langchain4j` module
- The tests will check, that new logger can be added and new header can be provided
- The tests should check both ordinary and streaming ("reactive") models
- The tests will affect Native mode, but not OpenShift

### Impact on resources

The added tests should have minimal impact on resources:

- There will be several new test methods, and possibly one new test class
- Tests will be executed on baremetal only

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

- Reviewed upstream pull request.
- Reviewed documentation

## Contacts

- Tester: Fedor Dudinskii fdudinsk@ibm.com

## References
- https://docs.quarkiverse.io/quarkus-langchain4j/dev/models.html#_customizing_model_builders