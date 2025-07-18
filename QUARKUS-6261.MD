# QUARKUS-6261 - Upgrade to Hibernate ORM 7, Hibernate Reactive 3 and Hibernate Search 8

JIRA: https://issues.redhat.com/browse/QUARKUS-6261

Upstream issues:

- https://github.com/quarkusio/quarkus/issues/42247
- https://github.com/quarkusio/quarkus/issues/38948
- https://github.com/quarkusio/quarkus/issues/43368 
- https://github.com/quarkusio/quarkus/issues/45164

Upstream PR: https://github.com/quarkusio/quarkus/pull/41310

Quarkus Hibernate extensions now use a newer version of the underlying Hibernate libraries, which brings improvements and breaking changes we need to cover.
The upstream PR is marked as breaking and noteworthy, and breaking changes are recorded in the Quarkus migration guide; therefore, the breaking changes are acceptable.

**Hibernate ORM 7.0 and Jakarta Persistence 3.2** (product status: supported):
- breaking changes: 
  - Minimum database versions were bumped to the oldest version still supported "by their respective vendors".
  - Some pluggable components cannot be CDI beans anymore, none of the pluggable components Quarkus QE use (like `org.hibernate.id.IdentifierGenerator`) was a CDI bean, therefore we are not affected.
  - `hibernate-jpamodelgen` annotation processor is now `hibernate-processor`
  - Schema and DDL changes 
      - some database-specific mapping between Java types and DB column types
      - quoting like for table names that was introduced in 7.0 is [now closer to previous behavior](https://hibernate.atlassian.net/browse/HHH-16516)
      - basic arrays are now mapped to database arrays instead of to binaries in native queries
  - Behavior changes
      - `quarkus.hibernate-orm.jdbc.statement-fetch-size` cannot be used for stateless sessions
      - misplaced annotations result in validation exceptions instead of being ignored
      - temporal types returned for native query results are now `java.time` instead of `java.sql`
      - stateless sessions now use second-level cache by default, and mode needs to be switched explicitly if users don't want to use this feature; this change was motivated by the introduction of Jakarta Data
  - API changes
      - various JDBC types were moved to internal packages and users are supposed to rely on Jakarta Persistence annotations, we already covered some of the changes, for example, we use the `org.hibernate.annotations.JdbcTypeCode` annotation in our `sql/hibernate` module,
      - various deprecations, changes, and removals for `Session`, `LockOptions`, `Metamodel`, `@Table`, or `@Proxy` classes. Hibernate-specific annotations like `@Cascade` or `@CascadeType` were deprecated in favor of JPA equivalents

**Hibernate Reactive 3.0** (product status: tech preview) - breaking changes and new features are inherited from Hibernate ORM, I didn't find anything interesting specific for Hibernate Reactive.
The only exception is changes to the `ReactiveIdentifierGenerator` API.

**Hibernate Search 8.0.0.Final** (product status: `Hibernate Search ORM + Elasticsearch ` is supported) - changes to the classes that are part of the public API are backwards compatible except for some incubating classes.
ElasticSearch max connection defaults have been increased. Important and relevant changes are static metamodel generation, projecting multivalued fields, and the ability to use DSL for requesting metric aggregations.

## Scope of the testing

- Hibernate ORM
  - new features
    - tracking the timestamp when using soft-delete
    - define prefixes for embedded types with `@EmbeddedColumnNaming`
    - new `@NamedEntityGraph` annotation for parsing a string representation of the graph
    - fetching of batch entities by id with `StatelessSession.getMultiple()` or `Session.findMultiple` (one should be enough)
    - typesafe replacements for hints, namely, we should test the lock and find options as they are useful in production
    - generating check constraints for enums using `AttributeConverter`, the interesting part here is that this is supposed to happen on startup, which you can hardly verify in vanilla Hibernate, so we should check it works and things are synchronized
    - enhanced support for JSON functions in HQL, we should add a smoke check, as we have no `json` HQL functions tested
    - JPA specific configuration properties `jakarta.persistence.sql-load-script-source` and `jakarta.persistence.schema-generation.database.action` can be used together to populate the database from a script. We need to test whether this script is added to the native executable as a resource and this feature works, especially in native.
    - `jakarta.persistence.SchemaManager` injection is newly documented, we should include it in our test coverage in some context
    - smoke test using 2nd-level cache for stateless session
      - add scenario using `@jakarta.persistence.Cacheable`
      - add scenario setting the cacheable hints in queries
  - changed behavior
    - mapping basic Java type arrays to database arrays instead of binary types
    - `java.time` temporal types returned for native query
    - recommended `Session` method switches like `load` -> `getReference`, `get` to `find`, `delete` to `remove`, `save` -> `persist`
  - database versions
    - bump tests using MariaDB 10.3 to the supported database version 10.11
    - bump tests using PostgreSQL 12 to the supported database version 13
- Hibernate Reactive
  - current coverage is sufficient, let's keep it as is
- Hibernate Reactive used together with Hibernate ORM
  - major and documented improvement is that now users can use Hibernate Reactive and Hibernate ORM together in one application, we should test it
  - I believe it makes sense to use this together with security, like most requests are handled on an IO thread until they interact with user code (like in an endpoint, identity provider, JAX-RS filter, or custom CDI interceptor)
  - let's try to use Security JPA Reactive while the application itself should do CRUD with Hibernate ORM, if that doesn't work, let's report the issue and fallback to custom coverage of both extensions together
- Hibernate Search 
  - using a static model in DEV mode to verify the annotation processor behavior
  - use DSL for requesting metric aggregations
  - projecting multivalued fields
- Elasticsearch
  - align Elasticsearch image version with upstream, bump the image version to `9.0.2`

## Existing test coverage

Quarkus QE Test Suite has sufficient test coverage for the previous Hibernate ORM, Reactive, and Search extensions.
Because most of the code is backwards compatible, we are already testing this bump, as can be seen from the issues and migrations we hit. 
Upstream adapted their existing test coverage to comply with the bumped versions; however, their test coverage in the Quarkus project wasn't enhanced.
For enhancements, they rely on the Hibernate projects non-Quarkus tests.

### Changes mentioned in the migration guide covered by Quarkus QE Test Suite

- we use MariaDB 10.3 image provided by Red Hat, so the fact that the minimal supported version is now 10.6 affected us; as a quick fix, [we set the `quarkus.datasource.db-version` configuration property to 10.3](https://github.com/quarkus-qe/quarkus-test-suite/pull/2506).
- we cover the switch from `hibernate-jpamodelgen` annotation processor to `hibernate-processor`, thanks to Jakarta Data test coverage
- we use the `org.hibernate.annotations.JdbcTypeCode` annotation in our `sql/hibernate` module which was the type mentioned in the JDBC types moved to internal packages
- changes to `ReactiveIdentifierGenerator` API affected our test coverage, which is [already adapted](https://github.com/quarkus-qe/quarkus-test-suite/commit/87cfee1a6d736046729fed45ed7bfa5d5c1a6a0b)
- we were affected by https://github.com/quarkusio/quarkus/issues/48476, and these tests are still disabled even though the issue is now fixed, so we need to enable these tests
- we use the `org.hibernate.annotations.GenericGenerator` class that has been deprecated and marked for removal in Hibernate Search 6.5, and we need to migrate it

### Selected changes mentioned in the migration guide not covered by this test plan or existing test coverage:

- `quarkus.hibernate-orm.jdbc.statement-fetch-size` cannot be used for stateless sessions, we have no test coverage for fetch size, and it seems like an advanced feature
- misplaced annotations results in validation exceptions instead of being ignored, which are not currently covered, and it seems like a developer experience improvement, not something for integration testing

### Impact on test suites and testing automation

Quarkus QE Test Framework needs to be adapted to support Hibernate Search annotation processors.
New tests will be added to the existing test classes in various `sql` database modules.
We will introduce a new Hibernate Search DEV mode test.
Tests will be executed in JVM, native, DEV, or OpenShift according to how the extended test classes are tested.

### Impact on resources

Test execution time should increase in a couple of minutes max, because we will try to only add new test methods, rather than new test classes.

## Getting familiar with the feature

The following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Hibernate ORM 7 on Quarkus: each new version brings a better database experience](https://quarkus.io/blog/hibernate7-on-quarkus/)
- [Migration Guide 3.24](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.24#jakarta-persistence-hibernate-orm)
- [Hibernate - What’s New in 7.0](https://docs.jboss.org/hibernate/orm/7.0/whats-new/whats-new.html)
- [Quarkus QE tracker for actions done on this feature](https://issues.redhat.com/browse/QQE-1716)
- [Hibernate - 7.0 Migration Guide](https://docs.jboss.org/hibernate/orm/7.0/migration-guide/migration-guide.html)
- [Hibernate Reactive 3.0.0.Final and 4.0.0.Beta1](https://in.relation.to/2025/05/26/hibernate-reactive-3_0_0_Final/)
- [Hibernate Search 8.0.0.Final: Migration Guide from 7.2](https://docs.jboss.org/hibernate/search/8.0/migration/html_single/)
- [Hibernate Search - 8.0 series - release](https://hibernate.org/search/releases/8.0/)
- [Updates to address Hibernate 7 update in Quarkus](https://github.com/quarkus-qe/quarkus-test-suite/pull/2496)
- [Query.setMaxResults results in wrong SQL queries against MSSQL/Postgres when using native queries](https://github.com/quarkusio/quarkus/issues/48476)
- [SQLServer2012Dialect replaced by SQLServerDialect](https://github.com/quarkus-qe/quarkus-test-suite/pull/2501)
- [Set explicitly min database version for older db images to fix OCP tests](https://github.com/quarkus-qe/quarkus-test-suite/pull/2506)
- [Hibernate Reactive 3.0 changes](https://github.com/hibernate/hibernate-reactive/issues?q=is%3Aissue%20is%3Aclosed%20label%3A3.0)
