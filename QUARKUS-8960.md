# QUARKUS-8960 - `Introduce skills`

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8960

Upstream PR:
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2251 - Introduce Skills (extension, `SkillsToolProvider`, sample, docs)
Related PRs:
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2654 - Introduce `@Skills` annotation
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2688 - Allow `@Skills` on AI service methods
- https://github.com/langchain4j/langchain4j/pull/4646 - upstream LangChain4j implementation of skills

## Feature summary

The PR adds new `quarkus-langchain4j-skills` extension which allows using LLM skills with Quarkus.
A skill is a collection of Markdown and (optional) resource files, located in a single directory, and contains instructions for LLM.
The extension loads skills from the directories listed in
`quarkus.langchain4j.skills.directories` (may contain paths on filesystem or Java classpath) and exposes them to the LLM as tools.The LLM decides when to activate a skill, so the instructions are loaded on demand instead of being part of every prompt.

## Scope of testing

- Loading skills from filesystem path and a `classpath:` path
- Wiring skills via the `@Skills` annotation on classes and methods
- Wiring skills via ToolProvider
- Marete checks

### Out of scope
- The skills loading and parsing logic inside LangChain4j itself (`Skills`, `ClassPathSkillLoader`, `FileSystemSkillLoader`),
  covered upstream in LangChain4j. We only test that the Quarkus integration drives it correctly.
- Shell mode — explicitly unsupported
- Integration with agents — this module is unsupported

### Target environments and modes
- JVM and native mode on baremetal and Openshift


## Existing coverage

Upstream `skills/deployment` contains `QuarkusUnitTest` based tests:
- `SkillsTest` — tools registered, `activate_skill` and `read_skill_resource` return expected content, `SkillsSystemMessageProvider`
- `SkillsFromClasspathTest` — `classpath:` directory
- `SkillsAnnotationTest` — `@Skills` filtering, `DefaultSkillsConfigurator`, unknown name fails
- `SkillsCustomConfiguratorTest` — checks custom programmatic configuration
- `SkillsAiServiceIntegrationTest` — checks, that `@Skill` annotation can limit skills, which is loaded for particular classes and methods 
- `SkillsChatMemoryTest` — system message is not duplicated across methods with different `@Skills`
- `SkillsUnknownSkillStartupFailureTest` — startup failure on unknown skill name
- `SkillsMultipleSourcesTest` — checks, that skills can be loaded from both classpath and absolute path 
- The sample (`samples/skills`) has no automated test, but uses features, which we are interested in.
We may try to reuse the app for tests inside Quarkus QE TS or add some tests in the sample itself.
It uses classpath loading and @Skills annotation on a class.

## Impact on test suites and automation

- New tests go into the existing `ai/langchain4j` module of the Quarkus test suite

## Impact on resources

- Estimated <10 new test methods across a few classes; JVM execution time impact of several minutes in the module.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:

* Reviewed upstream pull request.
* Reviewed documentation
* Run the skill sample manually, caught some errors and created a fix for it


## Contacts

- Tester: Fedor Dudinskii <fdudinsk@ibm.com>

## References

- https://redhat.atlassian.net/browse/QUARKUS-8960
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2251
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2654
- https://github.com/quarkiverse/quarkus-langchain4j/pull/2688
- https://github.com/langchain4j/langchain4j/pull/4646
- https://docs.quarkiverse.io/quarkus-langchain4j/dev/skills.html
- https://docs.langchain4j.dev/tutorials/skills
- https://agentskills.io
- https://github.com/quarkiverse/quarkus-langchain4j/tree/main/samples/skills
