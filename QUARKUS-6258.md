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

We also need to verify correct usage of OpenAI and watson.x APIs.

These features are covered in upstream tests (see below). 

## Getting familiar with the feature
The description: https://quarkus.io/blog/quarkus-meets-langchain4j/
Examples:
* https://github.com/quarkiverse/quarkus-langchain4j/
* https://quarkus.io/quarkus-workshop-langchain4j/

## Existing test coverage
See https://github.com/quarkus-qe/quarkus-test-plans/pull/237 for details

### Impact on test suite
We should focus on updating and running upstream tests on productised binaries, so no changes in  QE TS and QE FW are expected. 

## Impact on resources
The tests itself do not consume much resources (single ci.m1.medium/ci.m1.large should be enough).
Running the LLM model, on the other side, may consume a lot of resources, probably a running instance of xlarge is required.

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@ibm.com>