# QUARKUS-6258 Quarkus AI - phase 1 features

## Scope of testing
Verify features of Quarkus AI, which are going to be added in phase 1

### Test cases
The list of prioritized features (taken from the issue):

- Chat Model — self-explanatory, this feature is provided by most quarkus-langchain4j-$llmname extensions
- Chat memory (in-memory, SPI) — Ability of LLM to remember previous conversations. The guide: https://docs.quarkiverse.io/quarkus-langchain4j/dev/messages-and-memory.html#_memory_and_its_purpose
- System message — annotation, used together with chat models
- User message — ditto 
- Function Calling — aka tools. The guide: https://docs.quarkiverse.io/quarkus-langchain4j/dev/function-calling.html#
- Simple RAG (Naive/Frozen) — Retrieval Augmented Generation, getting information from user provided documents. The guide https://docs.quarkiverse.io/quarkus-langchain4j/dev/rag-easy-rag.html#
- Advanced RAG — https://docs.quarkiverse.io/quarkus-langchain4j/dev/rag-contextual-rag.html#
- Guardrails — https://docs.quarkiverse.io/quarkus-langchain4j/dev/guardrails.html#
- WebSocket chat integration:
  - The extension: https://quarkus.io/extensions/io.quarkiverse.langchain4j/quarkus-langchain4j-websockets-next/
  - Guide: https://docs.quarkiverse.io/quarkus-langchain4j/dev/websockets.html
- CDI Chat Model — related to chat memory: https://docs.quarkiverse.io/quarkus-langchain4j/dev/messages-and-memory.html#_cdi_scope_and_memory_isolation

We also need to verify that Quarkus AI apps can work properly with both OpenAI and Watson.x APIs (the latter is optional for the first release) and that it is integrated with observability (generate metrics, logs, and telemetry).
Additional security considerations:
- The tests should verify, that standard security annotations work as expected for Quarkus AI extensions.
- The tests should verify, that model can not use tools, which exist, but were not made available to it.

Pass criteria: for each of these features there is at least one test, which uses it to connect to LLM and get an answer. The answer looks like the query (and its context) was passed to LLM without losing data and the answer was retrieved without losing data either.

### Optional test cases
These are required only for test-preview status in case Quarkus will go the way of maximum support in the first release:
- Scoring Model
- Parsing of documents (PDF, Word, etc)
- MCP client and server (STDIO and Streamable HTTP )
- Testing AI infused application (required for dev preview)

## Getting familiar with the feature
The description: https://quarkus.io/blog/quarkus-meets-langchain4j/
Examples:
* https://github.com/quarkiverse/quarkus-langchain4j/
* https://quarkus.io/quarkus-workshop-langchain4j/

## Existing test coverage
### Workshop
https://quarkus.io/quarkus-workshop-langchain4j/section-1/step-11/  and  https://github.com/quarkusio/quarkus-workshop-langchain4j/tree/main/section-1/step-11
- Are not run automatically
- The tests use jlama, which requires Java 21+ (this means, we are unable to verify them with java17)
- If tests are run, they hang regularly, even on xlarge.

Given the above I do not recommend to (re)use this repository for product validation.

### Quarkiverse extension:
There are integration tests modules in the extension, but many of them use mocking or don't contain tests at all.
According to Mario, this is intentional, since they consider upstream lang4chain tests to be enough (but we do not!)
Ollama modules can be started on the laptop in dev mode. Jlama ones can be run as well.
The features we want to cover are being used in integration-tests modules for OpenAI (Chats, Chat Memory, user/system messages, Function calling/Tools, Guardrails) and RAG. There is no coverage for WebSockets (only in codestarts) and for ChatModel (only in mocktests).

## Impact on test suite
Given requirement to run tests with platform BOMs, we should focus on adding tests to Quarkus QE TS. Modules in Quarkiverse `intergration-tests` and `samples` folder can be used a source of inspiration, or, in the latter case, used directly with a help of `@GitRepositoryQuarkusApplication`.

Primary focus will be on RHEL-based testing.

As we aim for tech preview level for 3.27, we won't be focusing on full platform matrix coverage. We will be gaining experience to expand the coverage and platforms in the upcoming releases.
Several new modules (probably two: Basic and RAG) will be added, with running times ~3-5 minutes for JVM, ~5-7 for OCP, and around 10 minutes in Native mode for each.

### Expect list of RHBQ Jenkins jobs to be added/changed
#### For acceptance
- Startstop
- Quickstarts acceptance (using samples subdirectory from quarkus-langchain4j main repo)
- Marete (there would be a new platform BOM)
#### For general/extended verification
- Running samples tests on different platforms. Use existing quickstarts tests as an example.
- Running integration tests from langchain4j repo (if it is possible) on different platforms.
- Add langchain4j tests to the Quarkus QE TS, run these together with the rest of them. Main focus should be on OCP tests, since they are not covered in upstream (samples&integration) tests.

## Impact on test suites for Full Support
Fo full support, we need to run tests on all supported platforms (https://access.redhat.com/articles/4966181). To achieve this, we need to add as much coverage to Quarkus TS as possible, since it is the easiest way to run tests on OpenShift.
We also need to run some of the tests regularly on upstream langchain4j to check for regressions.

## Impact on resources
The tests themselves do not consume much (single ci.m1.medium/ci.m1.large should be enough).
Running the LLM model, on the other side, may consume a lot of resources, probably a running instance of xlarge is required.
Since we need to verify interoperability with OpenAI (and, later with WatsonX), we should make the tests use external APIs.
I expect resource consumption for tests to be roughly on par with existing http modules. 

## Budget considerations:
We have (on average) one minor release per two months (with general scope testing) or two major releases per year (which include extended scope testing).
For every major release, we run the extended scope approximately 5-10 times and the general scope the same number of times. For every minor release we run general testing 1 or 2 times.
OpenAI is priced based on the amount of tokens, where "token" is something like a root of a word[1].
The cheapest model[2] is gpt-4.1-nano with prices of $0.10 for 1M tokens of input($0.025 for cached input) and $0.40 for 1M tokens of output.
Workshop uses ~400 tokens of input. Output can be different, but let's assume 100 tokens per test as an upper limit.
That means, that one million runs of single workshop test will cost us 40 dollars for input and 40 for output, for a total cost of 80 dollars, and one dollar will allow us to do 10 000 runs.
If we put these tests into general testing (so, ~30 runs per year) and adding the debugging and development (100 runs altogether?), which means, that with current prices single dollar would be enough for approximately a century of testing.
Important notes:
Upstream uses gpt-4o-mini for testing, which costs roughly 1.5 times more, and the latest version of it (gpt-4.1-mini) costs four times more. It seems, that we may want to request ~10 USD worth of credits for the testing, and adjust it in the future. Another benefit for using cloud infrastructure is that it would be easier to use it during a move of infrastructure from Red Hat to IBM. 
Watson.x foundational models cost the same[3], but there is a free tier (with up to 50,000 tokens per month) which we may fit in and given, that we are part of IBM now, we may be able to get access from them.

## Non-automated testing

See https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6257.md#additional-requirements-for-full-support for details.

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@ibm.com>

## Footnotes
[1] https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them
[2] https://platform.openai.com/docs/pricing
[3] https://www.ibm.com/products/watsonx-ai/pricing#foundationmodelsibm
