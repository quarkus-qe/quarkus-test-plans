# QUARKUS-744 - Non application endpoints redirections

JIRA link: https://issues.redhat.com/browse/QUARKUS-744

The goal is to verify non-application redirections, in order to don't break users common use-cases.

## Scope of the testing

The following non-application endpoint will be covered:

```
- /metrics
     - /metrics/vendor
     - /metrics/application
    - /metrics/base
- /health
   - /health/live
   - /health/ready
  - /health/well
  - /health/group
- /openapi
- /swagger-ui
```

### Impact on testsuites and testing automation:
 - Ensure this coverage works on Openshift (JVM and NATIVE mode)

### Clients interactions

Redirections have a server point of view, where the server must respond with an HTTP 301 and a Location header, but also has a Client point of view, where those clients should follow the server redirect. 

There are two kinds of clients:
- Java HTTP Clients as Apache commons HttpClient, Microprofile RestClient, asynchronous Vertx HttpClient... etc. 
- Health Checks clients provided by a third entity as Kubernetes/Openshift readiness/liveness probe, custom health checks, or Prometheus, CloudWatch or other metrics tools... etc 

We will cover with this test plan the following clients:

- Java HTTP Clients
  - Microprofile sync/async HTTP Client
  - Vertx HTTP client
- Health check client
  - Quarkus 
  - k8s/Openshift
- Metrics
  - prometheus registry

### Security impact on redirections

Security must be another extra point to review. Specially non-application endpoint. We will cover Quarkus Keycloak AuthZ, in order to understand the impact on several flows. 

## Automated test development

- [Quarkus/non-application redirectiona scenario](https://github.com/quarkus-qe/quarkus-openshift-test-suite/tree/master/http/http-advanced): verify that all non-application endpoints are redirected, and checks that Http status is `301` and `Location` header is the expected one. Then verify that httpClient follow redirections.  