# QUARKUS-3260 - Tech Preview support for elasticsearch rest client

JIRA: https://issues.redhat.com/browse/QUARKUS-3260

Tech Preview Support for quarkus-elasticsearch-rest-client. RHBQ 3.2/Ghost release is targeting Tech Preview Support for elasticsearch rest client, there are development (and testing) plans that need to happen before full support.

Planned work upstream that might require breaking changes to configuration or the qualifiers of CDI beans (https://github.com/quarkusio/quarkus/issues/26991 in particular, but maybe also https://github.com/quarkusio/quarkus/issues/10905 and https://github.com/quarkusio/quarkus/issues/24011).

## Scope of the testing
 * Review coverage in https://github.com/quarkusio/quarkus and https://github.com/quarkusio/quarkus-quickstarts
 * Review https://quarkus.io/guides/elasticsearch and https://quarkus.io/guides/elasticsearch-dev-services guides
 * Experiment with standalone app using quarkus-elasticsearch-rest-client extension
 * Add test coverage into https://github.com/quarkus-qe/quarkus-test-suite
   * store and fetch data to/from Elasticsearch
   * stored data to include collections, basic data types, String with diacritics, date data types

### Impact on test suites and testing automation
 * New module will be introduced into https://github.com/quarkus-qe/quarkus-test-suite/tree/main/nosql-db
 * Tests targeting JVM and Native mode, dedicated OpenShift coverage is not necessary for TP level
 * Check for TP support level will be added into Marete test suite

### Impact on resources
 * Testsuite execution will be prolonged by 5-10 minutes, no additional CPU and memory requirements

## Topics outside the testing scope
 * Multiple instances of Elasticsearch
 * Tracing/OTel
   * Elasticsearch had its own instrumentation-based solution (APM) OTel was added recently to the backend
   * Usage of OTel in their rest client needs to be checked and exposed in Quarkus if available
 * Micrometer/metrics
   * Data would need to come from Apache HttpClient
   * There is https://github.com/open-telemetry/opentelemetry-java-instrumentation/tree/main/instrumentation/apache-httpclient approach which is agent based
 * Security
   * There are properties for user/password for basic auth
   * https://quarkus.io/guides/elasticsearch#programmatically-configuring-elasticsearch shows keystore use-case
   * Amazon OpenSearch Service uses signature-based authentication, and we have a way to do that in Quarkus with Hibernate Search: http://docs.quarkiverse.io/quarkus-hibernate-search-extras/dev/index.html#aws-integration. We have nothing of the sort for the rest client without Hibernate Search.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>
