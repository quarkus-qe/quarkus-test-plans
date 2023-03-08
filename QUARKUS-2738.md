# QUARKUS-2738 - Management port for Quarkus

JIRA link: https://issues.redhat.com/browse/QUARKUS-2738

This change allows to move some non-application endpoints to another host and port. The goal is to cover, that these endpoints work on both old and new port across Baremetal and OCP.

## Scope of the testing

The following non-application endpoints will be covered:

```
- /metrics
     - /metrics/vendor
     - /metrics/application
     - /metrics/base
- /health
```

### Scenarios that must be verified:


* Regression: 

```
# Application.properties example
quarkus.management.enabled=false # or without this option at all
```

Valid request example: `http://localhost:8080/q/health

* Default config (could be omitted from native tests to save time):

```
# Application.properties example
quarkus.management.enabled=true
```
Valid request examples: `http://localhost:9000/q/health` and `http://localhost:8080/q/dev`

* Custom port and SSL usage:
```
# Application.properties example
quarkus.management.enabled=true
quarkus.management.port=9002
quarkus.management.ssl.certificate.key-store-file=server-keystore.jks # http requests should not be processed 
quarkus.management.ssl.certificate.key-store-password=secret
```

Valid request example: `https://localhost:9002/q/health`

* Defined application non-application root path and management root path:

```
# Application.properties example
quarkus.http.root-path=/api
quarkus.http.non-application-root-path=/q
quarkus.http.redirect-to-non-application-root-path=false
quarkus.management.root-path=management
```

Valid request example: `http://localhost:9000/management/health` and `http://localhost:8080/api/q/dev`

NB: All options (except `quarkus.management.enabled` are build time, to verify that we should not put them into `application.properties`), but use overload functionality, from the framework(`withProperties`/`withProperty` methods)
### Impact on testsuites and testing automation:
 - Ensure this coverage works on Openshift (JVM and NATIVE mode)
 - New `quarkus.management.enabled` option os processed in a build time, we should add it as exception in the test framework
 - New tests will be added to `properties`,`monitoring/`, `http-advanced` and `http-advanced-reactive` modules.


## Advanced topics for test development

- Working with proxy
- Host customization

# Links
- https://issues.redhat.com/browse/QUARKUS-2738
- https://github.com/quarkusio/quarkus/pull/30506
- [QUARKUS-744.md](QUARKUS-744.md)
