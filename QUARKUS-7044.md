# QUARKUS-7044 - Full support for LangChain4J
Jira link: https://issues.redhat.com/browse/QUARKUS-7044

## Scope of testing
Verify, that all supported features in Quarkus Langchain4j/Quarkus AI work.

### Test cases
The list of supported features (from QUARKUS-6258 and QUARKUS-7044):
- Chat Model — self-explanatory, this feature is provided by most quarkus-langchain4j-$llmname extensions
- Chat memory (in-memory, SPI) — Ability of LLM to remember previous conversations. The guide: https://docs.quarkiverse.io/quarkus-langchain4j/dev/messages-and-memory.html#_memory_and_its_purpose
- System message — annotation, used together with chat models
- User message — ditto
- Function Calling — aka tools. The guide: https://docs.quarkiverse.io/quarkus-langchain4j/dev/function-calling.html#
- Guardrails — https://docs.quarkiverse.io/quarkus-langchain4j/dev/guardrails.html#
- WebSocket chat integration:
    - The extension: https://quarkus.io/extensions/io.quarkiverse.langchain4j/quarkus-langchain4j-websockets-next/
    - Guide: https://docs.quarkiverse.io/quarkus-langchain4j/dev/websockets.html
- CDI Chat Model — related to chat memory: https://docs.quarkiverse.io/quarkus-langchain4j/dev/messages-and-memory.html#_cdi_scope_and_memory_isolation
- AI services: https://docs.quarkiverse.io/quarkus-langchain4j/dev/ai-services.html
- Memory management: https://docs.quarkiverse.io/quarkus-langchain4j/dev/messages-and-memory.html

Test cases which should be run for release:
- Connecting to chat model (only OpenAI now)
- Passing User and System messages to it
- Passing chat history to the model
- Calling of external tools
- Guardrails
- Usage over Websockets protocol
- Registration of AI services

Some of these features has good coverage upstream and may not require running on OpenShift. In these cases, make sure, that these upstream tests are run in Native and on Windows and do not add them to the TS.

## Getting familiar with the feature
The description: https://quarkus.io/blog/quarkus-meets-langchain4j/
Examples:
* https://github.com/quarkiverse/quarkus-langchain4j/
* https://quarkus.io/quarkus-workshop-langchain4j/

## Impact on test suite

New tests should be added to `ai/langchain4j` folder in Quarkus QE TS or upstream into the langchain4j repo.
In the former case we should focus on Openshift tests.
Additionally, Marete configs should be updated for the following extensions:
- io.quarkiverse.langchain4j:quarkus-langchain4j-core
- io.quarkiverse.langchain4j:quarkus-langchain4j-openai
- io.quarkiverse.langchain4j:quarkus-langchain4j-websockets-next

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@ibm.com>
