# QUARKUS-1094 - Initial support of the gRPC in the dev ui

JIRA link: https://issues.redhat.com/browse/QUARKUS-1094

Quarkus has an experimental feature for gRPC in the DevUI. Now it is possible to test gRPC services provided by the server without implementing clients. The gRPC uses HTTP/2 which is well suited for microservices inner-communication. Even though browsers have support for HTTP/2, the browser's JS doesn't have an API to work with binary data frames (because everything in HTTP2 is binary).  
There are multiple solutions, the first one to use gRPC proxy (https://grpc.io/blog/state-of-grpc-web/#the-tech) and the second one, which the DevUI is using, is utilizing WebSockets. The gRPC request/response can be transcoded to WebSockets (according to https://cloud.redhat.com/blog/grpc-anywhere), but WebSockets are still utilizing HTTP/1.1.  
Under the hood when using DevUI with gRPC there is a connection upgrade happening `Upgrade: websocket` which is creating socket for `ws://localhost:8080/q/dev/grpc-test`. This socket endpoint is listening for JSON request messages with information about requested service name, method name and message.

## Scope of the testing
- Verify all gRPC services are listed in the DevUI
- Verify all gRPC services are properly identified by their type
- Verify Unary, Server Streaming, Client Streaming, Bidirectional Streaming are working properly


### Impact on testsuites and testing automation:
- Test will be implemented in https://github.com/quarkus-qe/quarkus-test-suite

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use

As part of feature exploration a GH issues and GH pull requests were filed:
 - https://github.com/quarkusio/quarkus/issues/19218
 - https://github.com/quarkusio/quarkus/pull/19206