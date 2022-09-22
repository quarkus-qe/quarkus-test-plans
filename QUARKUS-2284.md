# QUARKUS-2284 OpenShift Serverless is requesting that we support it when using OpenShift Serverless Functions

Jira: https://issues.redhat.com/browse/QUARKUS-2284

This feature is about productization of Quarkus Funqy and Quarkus Funqy Knative Events Binding extensions needed by OpenShift Serverless Functions.
We should create multiple Quarkus functions deployed with OpenShift CLI as a single application.
Functions should be used with Knative Eventing, test both native and JVM mode and verify Funqy features.
Before we can do that, we need to install Knative Eventing with our Quarkus QE Framework as we want the Test Suite to work without any prerequisites and the operator starts several pods that would keep running.

## Scope of the testing

Tests should verify:
- Quarkus function deployment using YAML file and OpenShift CLI (`oc`)
- Invoking Quarkus functions:
  - JSON object in the body of the request (HTTP POST request)
  - Data in the query string (HTTP GET request)
  - JSON object in the data property (CloudEvent)
- Quarkus function receives and processes:
  - bean (f.e. POJO)
  - `CloudEvent` using the Funqy `@Context` annotation
  - binary data
  - primitive type or its wrapper
- Reading and writing of the attributes of a `CloudEvent`
- Permitted return types:
  - `void`
  - `String`
  - `byte[]`
  - primitive type and its wrapper
  - Javabeans, maps, lists, arrays
  - `CloudEvents<E>`
  - `Uni<T>`, where `T` is any of types listed above
- Quarkus function accepts events from custom resource created by us as well as from Event Sources provided out of the box
- Cloud Event type and source matching:
  - default mapping when the type match the function name for the function to trigger
  - mapping changed by configuration within application.properties
  - mapping with `@CloudEventMapping` annotation
- Complex function chain, when one function triggers the next and so on
- DEV mode and unit testing using RestAssured using:
  - normal HTTP requests
  - Cloud Event Binary mode
  - Structured Mode
- Multiple functions declared in one application
- Accessing secrets and config maps from functions

### Impact on test suites and testing automation

- One new module will be added to our Test Suite:
  - Bare metal tests that cover DEV mode (JVM mode only)
  - OpenShift tests for Funqy applications deployed as OpenShift Serverless Functions in JVM and native mode

### Impact on resources:

- Impact on lab capacity is one medium size executor
- Required additional time for the test execution should be up to 14 minutes. Roughly we expect 5.5 min. for JVM mode and 8.5 min. for native mode:
  - ~ 2 minutes to install Knative Eventing
  - ~ 30 seconds to create broker and triggers
  - ~ 2/5 minutes to build and deploy a function in JVM/native mode
  - ~ 1 minute for uninstalling Knative Eventing

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Advanced topics for test development

- Interacting with a serverless application using HTTP2 and gRPC

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [OpenShift Serverless is requesting that we support it when using OpenShift Serverless Functions](https://issues.redhat.com/browse/QUARKUS-2284)
- [Serverless Functions and Quarkus Funqy](https://docs.google.com/document/d/1g44KBmXdgdOdY3gMycKFLrEuw7FtuZNvLp7L_x1XalY/edit#heading=h.8mijy7v3rllx)
- [Setting up OpenShift Serverless Functions](https://docs.openshift.com/container-platform/4.11/serverless/functions/serverless-functions-setup.html)
- [FUNQY KNATIVE EVENTS BINDING](https://quarkus.io/guides/funqy-knative-events)
- [Knative - About Brokers](https://knative.dev/docs/eventing/brokers/)
- [Brokers and Triggers](https://redhat-developer-demos.github.io/knative-tutorial/knative-tutorial/eventing/eventing-trigger-broker.html)
- [Installing Knative Eventing](https://docs.openshift.com/container-platform/4.11/serverless/install/installing-knative-eventing.html)
- [Accessing secrets and config maps from functions](https://docs.openshift.com/container-platform/4.11/serverless/functions/serverless-functions-accessing-secrets-configmaps.html)
- [Developing Quarkus functions](https://docs.openshift.com/container-platform/4.11/serverless/functions/serverless-developing-quarkus-functions.html)