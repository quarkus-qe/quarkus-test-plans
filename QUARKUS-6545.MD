# QUARKUS-6545 - Update to Hibernate ORM 7.1.0.Final, Hibernate Search 8.1.0.Final, Hibernate Reactive 3.1.0.Final

JIRA: https://issues.redhat.com/browse/QUARKUS-6545

Upstream PRs: 

- https://github.com/quarkusio/quarkus/pull/49344
- https://github.com/quarkusio/quarkus/pull/49429
- https://github.com/quarkusio/quarkus/pull/49982
- https://github.com/quarkusio/quarkus/pull/50053

Quarkus 3.26 arrives with updated Hibernate ORM, Reactive, and Search libraries. We need to identify relevant changes, ensure that breaking changes are properly documented, and test important new features.

* Hibernate ORM bump from 7.0 to 7.1 brings:
    * New features:
        * Resource Discovery in SE Environments - things like [referencing JAR files from persistence.xml](https://jakarta.ee/specifications/persistence/3.2/jakarta-persistence-spec-3.2#a12305) feels like something I’d not expect Quarkus to support or encourage
        * `org.hibernate.Locking` has been enhanced with Hibernate-specific options for the follow-on locking behavior and the PessimisticLockScope
        * Support for intercepting Session#merge events has been added to Hibernate’s Interceptor .
    * Changes to SPI (Pessimistic Locking, JSON mappings on Oracle 21+, enhancement option granularity) and API (Bytecode Enhancement Options, LockOptions, Session#getLobHelper)
    * Agroal Pool is now a transitive runtime dependency; therefore, hibernate-agroal users don’t need to add it explicitly
* Hibernate Reactive changes between 3.0 and 3.1 consist mostly of dependency bumps, DevOps tweaks, and minor fixes.
* Hibernate Search changes between 8.0 and 8.1 are:
    * Dependency upgrades (Hibernate ORM, Lucene, Elasticsearch, OpenSearch, Jackson, AWS SDK)
    * New Hibernate Search Platform BOMs - we do not consume these BOMs as we use Quarkus extensions instead.
    * Aggregation improvements
    * Minor changes and fixes

## Scope of the testing

* Hibernate Reactive - no test development needed, as changes can easily be covered by the Hibernate Reactive itself test coverage, and this extension is a tech preview.
* Hibernate Search:
    * Dependency upgrades are covered by existing test coverage.
    * Aggregation improvements will be tested by [new test development](https://github.com/quarkus-qe/quarkus-test-suite/pull/2656).
    * The Elasticsearch bump will be reflected by the [Elasticsearch version bump in our test suite](https://github.com/quarkus-qe/quarkus-test-suite/pull/2655)
* Hibernate ORM:
    * Fixes:
        * Bump should fix https://github.com/quarkusio/quarkus/issues/49593, we need to try to enable the test and follow-up (note: currently fails, I opened [issue 50298](https://github.com/quarkusio/quarkus/issues/50298))
        * [Classloader memory leak](https://github.com/quarkusio/quarkus/issues/46102) will be tested manually with a profiler
    * Changes to API and SPI are better suited for unit tests of the Hibernate team, not for Quarkus QE. They are mentioned in the Quarkus migration guide, and no TD on our side is required.
    * Enhancements:
        * Hibernate-specific locking options are already tested, and the scope and follow-on are minor enhancements; I do not plan to cover them, as they should behave the same in the vanilla Hibernate test coverage created by the Hibernate team.
        * The ability to intercept the Session#merge events is important as this feature is related to CDI and Hibernate integration with Quarkus, the [new test development will be added](https://github.com/quarkus-qe/quarkus-test-suite/pull/2659).

## Existing test coverage

Hibernate repositories have sufficient unit tests covering isolated Hibernate features.
Quarkus project test coverage was adapted, but not enhanced; the Hibernate team prefers to only add Quarkus specific tests.
Quarkus QE Test Suite covers dependency upgrades and backwards compatibility.

### Impact on test suites and testing automation

Existing test classes in the `hibernate/jakarta-data`, `hibernate/hibernate-fulltext-search`, and `hibernate/hibernate-orm` modules will be enhanced with a few test methods.

### Impact on resources

Negligible impact, the test execution time will increase by a couple of seconds.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Hibernate ORM 7.1.0.Final](https://github.com/hibernate/hibernate-orm/releases/tag/7.1.0)
- [Hibernate Search 8.1.0.Final is out](https://in.relation.to/2025/08/08/hibernate-search-8-1-0-Final/)
- [Hibernate Search 8.1.2.Final is out](https://in.relation.to/2025/09/09/hibernate-search-8-1-2-Final/)
- [Hibernate Reactive - 3.1.0.Final](https://github.com/hibernate/hibernate-reactive/releases/tag/3.1.0)
- [Bump Elasticsearch version to 9.1.4](https://github.com/quarkusio/quarkus/pull/50185)
- [Hibernate ORM - What’s New in 7.1](https://docs.jboss.org/hibernate/orm/7.1/whats-new/whats-new.html)
- [Hibernate ORM - 7.1 Migration Guide](https://docs.jboss.org/hibernate/orm/7.1/migration-guide/migration-guide.html)
- [Migration Guide 3.26](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.26#upgrade-to-hibernate-orm-7-1)
