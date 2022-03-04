# QUARKUS-1404 - Vert.x update to 4.2.x or 4.3.x

JIRA link: https://issues.redhat.com/browse/QUARKUS-1404

This test plan cover Vertx upgrade to 4.2

## External resources
- [Update to Vert.x 4.2.1 and related projects](https://github.com/quarkusio/quarkus/pull/21195)
- [Update to Vert.x 4.2.4 and Netty 4.1.73](https://github.com/quarkusio/quarkus/pull/23049)
- [What's new in Vert.x 4.2](https://vertx.io/blog/whats-new-in-vert-x-4-2/)

## Related test plans

 - [QUARKUS-190.md](QUARKUS-190.md)

## Scope of the testing

RHBQ testing should be focused on Quarkus specifics, not on Vert.x specific topic as there is Vert.x product to cover these aspects. 
4.2 introduces "Shared http client", but that's Vert.x specific feature. The suggested way for Quarkus is to use `smallrye-mutiny-vertx-web-client` and this library doesn't allow to create shared http client.
Another big feature is Websockets support, Quarkus delivers that using `quarkus-websockets` though.
Other noticeable features as related to Reactive SQL Clients, these topics are covered in dedicated Feature Requests.

No extra test development needed.
  
### Impact on testsuites and testing automation:

- All existing tests around `Vertx HttpClient`, `routes` and SQL `datasources` will cover backward compatibility issues related to these topics.
  
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features 
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>