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

There are five scenarios that must be verified:


* Relative paths: 

```
# Application.properties example
quarkus.http.root-path=/api
quarkus.http.non-application-root-path=q
quarkus.http.redirect-to-non-application-root-path=false
```

Valid request example: `http://localhost:8080/api/q/health`

* Empty non-application root path:

```
# Application.properties example
quarkus.http.root-path=/api
quarkus.http.non-application-root-path=/
quarkus.http.redirect-to-non-application-root-path=false
```
All request will return an HTTP status `404`

* Non-application root path with `slash` base path:
```
# Application.properties example
%emptyRootPath.quarkus.http.root-path=/
%emptyRootPath.quarkus.http.non-application-root-path=/q
%emptyRootPath.quarkus.http.redirect-to-non-application-root-path=false
```

Valid request example: `http://localhost:8080/q/health`

* Defined application base path and non-application root path:

```
# Application.properties example
quarkus.http.root-path=/api
quarkus.http.non-application-root-path=/q
quarkus.http.redirect-to-non-application-root-path=false
```

Valid request example: `http://localhost:8080/q/health`

* Backward compatibility (allow redirections):

```
quarkus.http.root-path=/api
quarkus.http.non-application-root-path=/q
quarkus.http.redirect-to-non-application-root-path=true
```

Valid request example: `http://localhost:8080/api/health`, will be redirected to `http://localhost:8080/q/health`

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

- [Quarkus/non-application redirection scenario](https://github.com/quarkus-qe/quarkus-openshift-test-suite/tree/main/http/http-advanced): verify that all non-application endpoints are redirected, and checks that Http status is `301` and `Location` header is the expected one. Then verify that httpClient follow redirections.  

- [Quarkus/non-application expected behavior scenario](https://github.com/quarkus-qe/beefy-scenarios/tree/main/020-quarkus-http-non-application-endpoints): verify that all non-application endpoints and application base path works as expected with several configuration.

