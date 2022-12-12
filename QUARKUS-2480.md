# QUARKUS-2480 - Introduce @SearchExtension to configure Hibernate Search through annotated beans

JIRA link: https://issues.redhat.com/browse/QUARKUS-2480

Quarkus documentation: https://quarkus.io/guides/hibernate-search-orm-elasticsearch#analysis-configurer
Related to: [QUARKUS-1223](QUARKUS-1223)

Hibernate `@SearchExtension` annotation allows you to customize your Hibernate/Elasticsearch index configuration

## Scope of the testing

There are two ways to provide your Elasticsearch index configuration:
 - `@SearchExtension` annotation, that is [already covered in quickstart](https://github.com/quarkusio/quarkus-quickstarts/blob/main/hibernate-search-orm-elasticsearch-quickstart/src/main/java/org/acme/hibernate/search/elasticsearch/config/AnalysisConfigurer.java#L8) 
 - `quarkus.hibernate-search-orm.elasticsearch.analysis.configurer` property

Test developement will be focused on the application.properties way. We will use an [existing scenario](QUARKUS-1223.md) in Quarkus test suite in order to configure the analyzer through a custom bean that will be injected into the application through a property in the application.properties.

Review corner cases as checking error messages when `@SearchExtension` annotation and `application.properties` is misconfigured.

## Impact on test suites / test environment / reources

The impact is going to be minimum because is going to use an [existing coverage](QUARKUS-1223.md), so in terms of resources we don't have anything else to add to the existing impact on [Hibernate Search product support](QUARKUS-1223.md). 

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use
- Review upstream coverage

## Advanced topics for test development

- No advanced topics found

## Contacts

* Tester: Pablo Gonzalez <pagonzal@redhat.com>