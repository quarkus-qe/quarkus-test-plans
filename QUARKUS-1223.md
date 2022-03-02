# Hibernate Search product support

JIRA link: https://issues.redhat.com/browse/QUARKUS-1223

Project webpage: https://hibernate.org/search/  
Community documentation of the project: https://docs.jboss.org/hibernate/stable/search/reference/en-US/html_single/  
Community documentation of the Quarkus extension: https://quarkus.io/guides/hibernate-search-orm-elasticsearch  

Hibernate Search is provided by community Quarkus' extension `hibernate-search-orm-elasticsearch`.  
Only Elasticsearch/OpenSearch backend is supported by design (no Lucene).  
It integrates with Hibernate ORM. Integration with Hibernate Reactive is not implemented.  
The extension works well with RESTEasy Classic and RESTEasy Reactive.

## Scope of testing
Testing should include following topics:
- Search DSL
- Indexes:
  - `@FullTextField`
  - `@KeywordField`
  - Embedded indexes using `@IndexedEmbedded`
- Setting up analyzers
- Initial indexing using `MassIndexer`
- Elasticsearch backend
- OpenSearch backend
- Multiple persistence units
- Outbox polling
- Multi-tenancy
- Dev Services


### Existing test coverage
Solid coverage exists in upstream resources for most of the outlined topics.  
All tests use Hibernate Search in combination with RESTEasy Classic: `quarkus-resteasy`, `quarkus-resteasy-jackson`, `quarkus-resteasy-jsonb`.  
Combinations with databases: `quarkus-jdbc-postgresql`, `quarkus-jdbc-h2`.  

(Following list mentions only features not present in other resources.)  
- Integration tests in Quarkus upstream
  - https://github.com/quarkusio/quarkus/tree/main/extensions/hibernate-search-orm-elasticsearch/deployment/src/test/java/io/quarkus/hibernate/search/elasticsearch/test
    - Multiple persistence units
  - https://github.com/quarkusio/quarkus/tree/main/integration-tests/hibernate-search-orm-elasticsearch-coordination-outbox-polling
    - Outbox polling
  - https://github.com/quarkusio/quarkus/blob/main/integration-tests/hibernate-search-orm-elasticsearch-tenancy
    - Multi-tenancy using separate schema
  - https://github.com/quarkusio/quarkus/tree/main/integration-tests/hibernate-search-orm-elasticsearch
    - Dev Services
    - Embedded indexes using `@IndexedEmbedded`
  - https://github.com/quarkusio/quarkus/blob/main/integration-tests/hibernate-search-orm-opensearch
    - OpenSearch backend
- Quickstarts: https://github.com/quarkusio/quarkus-quickstarts/tree/main/hibernate-search-orm-elasticsearch-quickstart
    - Initial indexing using `MassIndexer`

What is missing:
- Use of RESTEasy Reactive.
- Use of other supported databases.

## Automated test development
- Enable relevant Quickstart module in Quickstart acceptance job: https://github.com/quarkusio/quarkus-quickstarts/tree/main/hibernate-search-orm-elasticsearch-quickstart.
- Implement new test scenario in QE test suite (include OpenShift tests) combining:
  - Multiple persistence units, outbox polling and multi-tenancy (separate schema).
  - RESTEasy Reactive.
  - Selection of other supported databases: `jdbc-mariadb`, `jdbc-oracle`.

## Impact on test suites and test environment
- No modifications in test environment expected.
- 1 new module in QE test suite: 2 / 5 / 5 / 10 minutes increase (JVM / native / OpenShift / OS native).
- 1 module added to Quickstarts acceptance job: ~1 minute increase.
