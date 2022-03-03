# QUARKUS-1405 - Mutiny 1.x improvements

JIRA link: https://issues.redhat.com/browse/QUARKUS-1405

This test plan cover Mutiny 1.x improvements, more in detail the concept of mutiny [context](https://smallrye.io/smallrye-mutiny/guides/context-passing).

## Scope of the testing

Mutiny contexts shall be primarily used to share transient data used for networked I/O processing such as correlation identifiers, tokens, etc. They should not be used as general-purpose data structures that are frequently updated and that hold large amounts of data.

Test development will focus on cover the notion of mutiny `context` and how to pass this metadata values between several components.

Custom objects as `context` are supported and must be tested as a part of this test development.

### Impact on testsuites and testing automation:

Reusing some existing scenarios as `http/rest-client-client-reactive` where are involved two or more components, could be a good approach. In the end, the most important is to be sure that the context is propagated between several components.  

### Advanced topics for test development

- Verify if there is any limit in terms of amount of data that you can save/transfer as a context. 
- Verify if the context is really threadsafe, not only in terms of thread concurrency (two threads trying to update the same key at the same time), also in terms of race condition, for example, if we have a collision how does the context manage this collision, maybe by Optimistic concurrency strategy.  
- Verify if Mutiny context has any impact in HttpClient tracing headers propagation. And if there is any collision between "standard headers" and developer header, how this ciollision is managed. This topic could be related to [istio/b3 headers](https://istio.io/latest/about/faq/distributed-tracing/#initial-zipkin-header)

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>