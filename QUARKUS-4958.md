# QUARKUS-4958 - Update Spring APIs to Spring Boot 3

JIRA: https://issues.redhat.com/browse/QUARKUS-4958

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/40344
- https://github.com/quarkusio/quarkus/pull/41206
- https://github.com/quarkusio/quarkus/pull/41993

Spring Data and Spring Data REST [has been updated](https://github.com/quarkusio/quarkus-spring-data-api/pull/9)
to the 3.2.5 and 4.2.5 respectively.
Quarkus supports Spring interfaces only in documented use cases.
That is, this test plan is only interested in a Spring API changes supported by Quarkus.
Following changes are relevant:
- [New CRUD repository interfaces that return List instead of Iterable](https://github.com/spring-projects/spring-data-commons/wiki/Spring-Data-2022.0-(Turing)-Release-Notes#new-crud-repository-interfaces-that-return-list-instead-of-iterable)
- [Sorting repositories no longer inherit from CRUD repositories](https://github.com/spring-projects/spring-data-commons/wiki/Spring-Data-2022.0-(Turing)-Release-Notes#sorting-repositories-no-longer-inherit-from-crud-repositories)
- [JpaRepository.getReferenceById replaces JpaRepository.getById](https://github.com/spring-projects/spring-data-commons/wiki/Spring-Data-2021.2-(Raj)-Release-Notes#jparepositorygetreferencebyid-replaces-jparepositorygetbyid)

We are not really interested in other Spring changes and features (as support for them is not documented).
Breaking changes goes mostly down to the interface hierarchy changes and excessive refactoring inside Quarkus Spring Data JPA deployment module.

## Scope of the testing
Following additions should be tested:
- `getReferenceById` as replacement for deprecated `getOne` and `getById`
- `ListCrudRepository` and `ListPagingAndSortingRepository` new interfaces
- test above-mentioned with `@Transactional` as that is missing in our existing test coverage
- test `Sort` and `TypedSort` in native as newly, [there is related native image substitution](https://github.com/quarkusio/quarkus/blob/main/extensions/spring-data-jpa/runtime/src/main/java/io/quarkus/spring/data/runtime/Target_org_springframework_data_domain_Sort_TypedSort.java)

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
Quarkus QE Test Suite has existing test coverage that was affected by these breaking changes.
This required adjustments:
- https://github.com/quarkus-qe/quarkus-test-suite/pull/1835

And allowed us to detect issues:
- https://github.com/quarkusio/quarkus/issues/41134
- https://github.com/quarkusio/quarkus/issues/41135
- https://github.com/quarkusio/quarkus/issues/41136
- https://github.com/quarkusio/quarkus/issues/41134
- https://github.com/quarkusio/quarkus/issues/41292

Upstream test coverage is sufficient and tests were added for changes and fixed reported bugs.
My only concern is strange lifecycle of these tests.
I can see that transaction interceptors are applied on unit test methods.
I can't foresee if there can be issues that are not detected thanks to this fact, like class loading etc.
Hence, it is important to test new additions with transactions.

### Impact on test suites and testing automation
Couple of unit test methods calling newly added endpoints shall be added to the Quarkus QE Test Suite Spring Data module.

### Impact on resources
Couple of seconds added to the overall execution time.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [Quarkus QE tracker](https://issues.redhat.com/browse/QQE-930)
- [Spring compatibility layer aligned with Spring Boot 3](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.12#spring-compatibility-layer-aligned-with-spring-boot-3)
