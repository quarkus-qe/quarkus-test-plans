# QUARKUS-2736 - Quarkus 3.x should use Jakarta EE 10 APIs

JIRA: https://issues.redhat.com/browse/QUARKUS-2736

PR: https://github.com/quarkusio/quarkus/pull/30856 (mainly)

Jakarta EE 10 has been integrated with the Quarkus main branch since the Quarkus 3.0.0.Alpha4.
The most visible change is renaming of the packages from `javax` to `jakarta`, but
Jakarta EE 10 comes with new features added to the specifications, and their implementations.
The Quarkus codebase has been adjusted, mainly Quarkus ArC, RESTEasy Classic and RESTEasy Reactive.
[There is a straightforward way](https://quarkus.io/blog/quarkus-3-0-0-alpha4-released/#upgrading-to-quarkus-3) to migrate simple Maven projects using a JBang script.
For most things, migration to Jakarta EE 10 doesn't cause any issues, however Quarkus experienced some issues due to dependencies
that didn't migrate to Jakarta yet. For example [REST Assured only worked with Jackson](https://github.com/quarkusio/quarkus/issues/29225),
not with JSON-B, as there were no version of the REST Assured supporting the Jakarta packages.

Probably the most significant breaking change is [migrating to the Jakarta Persistence Provider](https://github.com/hibernate/hibernate-orm/blob/6.0/migration-guide.adoc#jakarta-persistence) - Hibernate 6.2.
Quarkus is still going to target Jakarta EE 9 in certain areas, [Angus Jakarta Mail](https://github.com/quarkusio/quarkus/issues/26989) is an excellent example.
It is important to keep in mind that QUARKUS-2736 is about [migrating Java EE APIs already implemented by Quarkus](https://github.com/quarkusio/quarkus/issues/26990)
to the Jakarta, not making Quarkus fully compliant with the Jakarta EE 10 specification.
However, there is ongoing effort to draw nearer specification in certain areas, such as [CDI Lite compliance (CDI 4.0)](https://github.com/quarkusio/quarkus/issues/28558).
Quarkus deliberately fails to meet these specification requirements, that doesn't go well with Quarkus principles and concept.
For example, Quarkus RESTEasy implementation [choose to disable default exception mappers](https://github.com/quarkusio/quarkus/blob/main/extensions/resteasy-classic/resteasy-common/deployment/src/main/java/io/quarkus/resteasy/common/deployment/ResteasyCommonProcessor.java#L159) as Vert.X error handling provides greater capabilities.

## Scope of testing

Tests should verify:

- there is no regressing caused by the migration
- compliance with specification in areas that Quarkus integrations decided to meet, for example:
  - [Hibernate 6.2](https://github.com/quarkusio/quarkus/issues/26986):
    - defaults of the Jakarta Persistence `@SequenceGenerator` annotation are respected; previously implicit sequences, like the `hibernate_sequence` did not
  - [RESTEasy Reactive support](https://github.com/quarkusio/quarkus/issues/27570) of the JAX-RS 3.1.0:
    - Jakarta API changes in `jakarta.ws.rs.ext.RuntimeDelegate` added new methods `createConfigurationBuilder`, `bootstrap`, 
      `createEntityPartBuilder` that [Quarkus decided to not support for now](https://github.com/quarkusio/quarkus/blob/main/independent-projects/resteasy-reactive/common/runtime/src/main/java/org/jboss/resteasy/reactive/common/jaxrs/RuntimeDelegateImpl.java#L123).
      We need to consider implications and confirm there is no significant impact on customers.
  - elder JAX-RS specification versions worked with concept of unmapped exceptions
    (exceptions without default mapper), however [now](https://github.com/resteasy/resteasy/pull/3044), there is 
    [always at least one mapper](https://github.com/jakartaee/rest/blob/3.0.0/jaxrs-spec/src/main/asciidoc/chapters/providers/_exceptionmapper.adoc)
    for each exception. On the other hand, MicroProfile requires unmapped exceptions to be counted as failed metrics.
    We should evaluate and test [impacts of this change](https://github.com/quarkusio/quarkus/pull/30856/commits/becd0f493e9de97bf628cd312ed8e06d1a9d3a1e).
    In order to verify related failures (e.g. [1](https://github.com/quarkusio/quarkus/issues/27809), [2](https://github.com/quarkusio/quarkus/issues/27808)), we should check:
    - default RESTEasy exception [mappers are disabled](https://github.com/quarkusio/quarkus/blob/main/extensions/resteasy-classic/resteasy-common/deployment/src/main/java/io/quarkus/resteasy/common/deployment/ResteasyCommonProcessor.java#L159)
      and error is handled by `QuarkusErrorHandler` or any Vert.X error handler with higher priority
    - SmallRye Health metrics for unmapped RESTEasy exceptions (responses with HTTP status 500)
  - `jakarta.websocket.ClientEndpointConfig` added `getSSLContext`, with which our [BearerTokenClientEndpointConfigurator](https://github.com/quarkusio/quarkus/blob/main/extensions/websockets/client/runtime/src/main/java/io/quarkus/websockets/BearerTokenClientEndpointConfigurator.java)
    deals simply by returning `null`. We should confirm [upstream discussion](https://github.com/quarkusio/quarkus/pull/27732/files)
    that this method is unlikely to be used
  - determine coverage on Arc compliance with CDI Lite specification:
    - [CDI 4.0.1 introduces some new methods and classes](https://github.com/quarkusio/quarkus/issues/27574), 
      and we make sure ArC changes are backwards compatible or documented
    - [Using @Typed with illegal values should throw an exception](https://github.com/quarkusio/quarkus/pull/30452)
    - [Class based bean should not be able to retain wildcard as its bean type](https://github.com/quarkusio/quarkus/pull/30447)
    - filed injection points with `@Named` without value should use `@Named(fieldName)` as the qualifier and other
      [various compatibility fixes](https://github.com/quarkusio/quarkus/pull/30509)
    - and others: [1](https://github.com/quarkusio/quarkus/pull/30544), [2](https://github.com/quarkusio/quarkus/pull/30666),
      [3](https://github.com/quarkusio/quarkus/pull/30917), [4](https://github.com/quarkusio/quarkus/pull/30924), 
      [5](https://github.com/quarkusio/quarkus/pull/30950), [6](https://github.com/quarkusio/quarkus/pull/31248),
      [7](https://github.com/quarkusio/quarkus/pull/31444)
  - migration to Jakarta Authorization API 2.1.0 and Authentication API 3.0.0 [seen implemented changes in Elytron Security](https://github.com/quarkusio/quarkus/issues/27643)
    backwards compatibility needs to be verified
- we have or add coverage for new features based on specification:
  - [Jakarta REST Services 3.1](https://github.com/quarkusio/quarkus/pull/31124) added support for SameSite cookies
  - Leverage RESTEasy Classic CDI and [use @Inject instead of the deprecated @Context in RESTEasy Classic](https://github.com/quarkusio/quarkus/issues/29240)
- several [MP TCK failures](https://github.com/quarkusio/quarkus/issues/27513) are expected and comes from temporary 
  incompatibility with Jakarta EE 9 and 10, or are early resolved to avoid impact on customers
- we have necessary coverage on [MicroProfile 6.0 migration](https://github.com/quarkusio/quarkus/issues/31084) that is
  based on Jakarta EE 10 Core Profile, to make sure there is no transient impact
- ensure good user experience and simplicity of migration to Hibernate ORM 6.2:
  - add coverage for [configuration property to work with database schemas generated for Hibernate ORM 5.6](https://github.com/quarkusio/quarkus/pull/31540)
    as portion of these changes is related to Jakarta specification
  - add coverage for `quarkus.datasource.db-version` [configuration property](https://github.com/quarkusio/quarkus/pull/31701)
  - Hibernate ORM is lenient on configuration properties prefixed with `javax.persistence`, however all features are not
    guaranteed to work with `javax`, therefore we should [encourage in upstream issue](https://github.com/quarkusio/quarkus/issues/31130)
    to log warning or fail. This way, users are going to be warned early.
- our test suites uses terminology [compliant to the specification](https://github.com/quarkusio/quarkus/pull/31700/files), for example:
  - replace `JPA` with `Jakarta Persistence`
  - replace `JAX-RS` with `Jakarta REST implementation` or `Jakarta REST`

### Existing tests

Naming migration of packages and imports from `javax` to `jakarta` has already been done, there was no
regression caused by the migration, however we had to disable temporarily several tests using Quarkiverse extensions that don't have
Jakarta version yet. Such examples are Quarkus Artemis, Kogito, Quarkus Pact and Quarkus Camel. Similarly, we disabled
tests using external applications like Quarkus Quickstarts, Quarkus TODO, Quarkus Workshop etc. Enabling this test will
sufficiently prove there is no regression.

ArC changes are verified by [newly enabled ArC TCK tests](https://github.com/quarkusio/quarkus/pull/31589) that are run
as part of the upstream CI.

Our tests that use Hibernate 6.2 all [required changes](https://github.com/quarkus-qe/quarkus-test-suite/pull/1082) 
regarding sequence generation, dialects and some also needed schema migration changes. Our Quarkus Jdk specifics Test Suite
cover changes in Hibernate Query Language semantics not related to Jakarta as proved by [required changes](https://github.com/quarkus-qe/quarkus-jdkspecifics/pull/62).
It's safe to say Quarkus Test Suite covers major Hibernate 6.2 breaking changes related to Jakarta EE 10 migration.

Default exception mappers introduced by RESTEasy Classic are covered upstream as the mappers were actually disabled in
reaction to failing tests.

Elytron Security migration is sufficiently covered by Quarkus QE Test Suite, Quarkus Quickstarts run as part of upstream CI
and upstream integration modules.

## Impact on test suites and test environment

Beefy Scenarios should cover newly introduced configuration properties that enforces legacy behavior and provides
smooth migration.

The SameSite cookies added by Jakarta REST Services 3.1 will require one unit test with multiple HTTP requests.
Possibility to use `@Inject` introduced RESTEasy Classic CDI will be done by migration part of injections done with `@Context`.
For example, we will replace annotation on the injection point of the `UriInfo`.

All test suites should be refactored to use new names (Jakarta Persistence, Jakarta REST).

### Manual verification

RESTEasy Reactive changes regarding `jakarta.ws.rs.ext.RuntimeDelegate` additions should be inspected manually
as they are exclusively dummy methods that throw `UnsupportedOperationException` and are only used in rare scenarios.

We need to assess possible use cases for `getSSLContext` of the `BearerTokenClientEndpointConfigurator` just to be sure 
that it is safe to return `null` and do nothing.

### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be below 4 minutes as we expect but a small code additions
- No measurable additional requirements for a Native image scenario

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Our (bumpy) road to Jakarta EE 10](https://quarkus.io/blog/our-bumpy-road-to-jakarta-ee-10/)
- [Quarkus 3.0.0.Alpha1 released](https://quarkus.io/blog/quarkus-3-0-0-alpha1-released/)
- [Quarkus 3.0.0.Alpha2 released](https://quarkus.io/blog/quarkus-3-0-0-alpha2-released/)
- [Quarkus 3.0.0.Alpha3 released](https://quarkus.io/blog/quarkus-3-0-0-alpha3-released/)
- [Quarkus 3.0.0.Alpha4 released](https://quarkus.io/blog/quarkus-3-0-0-alpha4-released/)
- [Quarkus 3.0.0.Alpha5 released](https://quarkus.io/blog/quarkus-3-0-0-alpha5-released/)
- [Migration Guide 3.0](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.0)
- [Migration Guide 3.0: Hibernate ORM 5 to 6 migration](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.0:-Hibernate-ORM-5-to-6-migration)
