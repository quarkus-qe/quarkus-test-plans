# QUARKUS-6258 Quarkus AI - phase 1 features

## Scope of testing
Verify features of Quarkus AI, which are going to be added in phase 1

### Test cases
The list of the features are taken from the issue:

Chat Model 	
Chat memory (in-memory, SPI) 	
System message 	
User message 	
Function Calling 	
Simple RAG (Naive/Frozen) 
Advanced RAG
Guardrails 	
WebSocket chat integration 	
CDI Chat Model.

We also need to verify correct usage of OpenAI and watson.x APIs as well as observability (metrics, logging and telemetry).

## Getting familiar with the feature
The description: https://quarkus.io/blog/quarkus-meets-langchain4j/
Examples:
* https://github.com/quarkiverse/quarkus-langchain4j/
* https://quarkus.io/quarkus-workshop-langchain4j/

## Existing test coverage
### Workshop
https://quarkus.io/quarkus-workshop-langchain4j/step-11/#running-the-llm-inference-locally  and  https://github.com/quarkusio/quarkus-workshop-langchain4j/tree/main/step-11
Are not run automatically
The tests use jlama, which requires Java 21+ (this means, we are unable to verify them with java17)
If tests are run, they hang regularly, even on xlarge.
Given the above I do not recommend to (re)use this repository for product validation.

### Quarkiverse extension:
There are integration tests modules in the extension, but many of them use mocking or doesn't contain tests at all.
According to Mario, this is intentional, since they consider upstream lang4chain tests to be enough (but we do not!)
Ollama modules can be started on the laptop in dev mode. Jlama ones can be run as well.
The features we want to cover are being used in integration-tests modules for openai (Chats, Chat Memory, user/system messages, Function calling/Tools, Guardrails), rag,. There is no coverage for WebSockets (only in codestarts) and for ChatModel (only in mocktests).

### Impact on test suite
We should focus on updating and running upstream tests on productised binaries. Since we need to run tests on Openshift, we have to add tests in Quarkus TS, but try to use source code from https://github.com/quarkiverse/quarkus-langchain4j/tree/main/integration-tests inside them via `@GitRepositoryQuarkusApplication`. If that fails, just create ordinary tests in the TS.
We will also need to separate these modules, so we only run them during the prod testing.

## Impact on resources
The tests itself do not consume much resources (single ci.m1.medium/ci.m1.large should be enough).
Running the LLM model, on the other side, may consume a lot of resources, probably a running instance of xlarge is required.
Since we need to verify interoperability with OpenAI and WatsonX, we need to either find ways to run models with compatible APIs on our hardware or request credits to use them in cloud.

## Budget considerations:
We have (on average) one minor release per two months (with general scope testing) or two major releases per year (which include extended scope testing).
For every major release we run extended scope approximately 5-10 times and general scope the same amount of times. For every minor release we run general testing 1 or 2 times.
OpenAI is priced based on amount of tokens, where "token" is something like a root of a word[1].
The cheapest model[2] is gpt-4.1-nano with prices of $0.10 for 1M tokens of input($0.025 for cached input) and $0.40 for 1M token of output.
Workshop uses ~400 token of input. Output can be different, but let's assume 100 tokens per test as an upper limit.
That means, that one million runs of single workshop test will cost us 40 dollars for input and 40 for output, for a total cost of 80 dollars, and one dollar will allow us to do 10 000 runs.
If we put these tests into general testing (so, ~30 runs per year) and adding the debugging and development (100 runs altogether?), which means, that with current prices single dollar would be enough for approximately a century of testing.
Important notes:
Upstream uses gpt-4o-mini for testing, which costs roughly 1.5 times more, and the latest version of it (gpt-4.1-mini) costs four times more. It seems, that we may want to request ~10 USD worth of credits for the testing, and adjust it in the future.
Watson.x foundational models cost the same[3], but there is a free tier (with up to 50,000 tokens per month) which we may fit in and given, that we are part of IBM now, we may be able to get access from them.

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@ibm.com>

##Footnotes
[1] https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them
[2] https://platform.openai.com/docs/pricing
[3] https://www.ibm.com/products/watsonx-ai/pricing#foundationmodelsibm