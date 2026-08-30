# QUARKUS-7820 - `quarkus.package.jar.tree-shake` option to eliminate unused classes from JARs

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7820

Upstream PR: https://github.com/quarkusio/quarkus/pull/53295

Documentation: https://quarkus.io/guides/jar-tree-shaking

This feature adds build-time tree-shaking to Quarkus JAR packaging.
When enabled, Quarkus analyses bytecode reachability from the application classes and excludes unreachable dependency classes from the produced JAR, reducing its size.
It is disabled by default, enabled with `quarkus.package.jar.tree-shake.mode=classes` (default `none`), and applies to `fast-jar`, `uber-jar`, `legacy-jar` and `aot-jar` but not `mutable-jar`.
It will be productized as tech preview for 3.40.

## Scope of the testing

Verify that applications still start and behave correctly after tree-shaking.
Testing will use a new `packaging/tree-shake` module, modelled on `packaging/jar`.

Tests will cover:

* An application returns the same responses with `tree-shake.mode=classes` as with `none`, built as `fast-jar`, `uber-jar`, `legacy-jar` and `aot-jar`.
* Each JAR type is smaller with `tree-shake.mode=classes` than with `none` for the same application.
* After tree-shaking, a JWT-secured endpoint, a Hibernate/JDBC query, a Qute template, the OpenAPI document and application logging all still work.
* A dependency listed in `quarkus.package.jar.tree-shake.excluded-artifacts` keeps all its classes and the datasource-backed endpoint still returns data, verified by comparing the dependency's `.class` entry count in the produced JAR against the unshaken `none` build.
* Enabling tree-shaking on `mutable-jar` does not break the build or the application.

## Existing test coverage

Upstream integration test: https://github.com/quarkusio/quarkus/blob/main/integration-tests/maven/src/test/java/io/quarkus/maven/it/TreeShakeIT.java

`TreeShakeIT` asserts, per JAR type, which classes are kept or removed, using purpose-built libraries.
Upstream does not run a real multi-extension application with tree-shaking enabled, nor on OpenShift.
The QE test suite has no tree-shaking coverage; it tests JAR packaging in `packaging/jar` and `packaging/quarkus`.

### Impact on test suites and testing automation

A new `packaging/tree-shake` module will be added, modelled on `packaging/jar`.
The application is built with tree-shaking enabled for each JAR type (selected via `quarkus.package.jar.type`) and its endpoints asserted against a `none` baseline.
As the feature is disabled by default, existing modules are unaffected.

### Impact on resources

Tests will be executed on baremetal in JVM mode.
Enabled builds are slower, as tree-shaking adds a build step and forks a JVM during analysis.
The estimated execution time increases by 7 minutes.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Reviewed the upstream pull request, the `jar-tree-shaking` guide, and the `TreeShakeConfig` configuration
- Focus on exploratory testing of the feature

## Contacts

* Tester: Slimane Abzar <slimane.abzar@ibm.com>

## References

- https://github.com/quarkusio/quarkus/pull/53295
- https://quarkus.io/guides/jar-tree-shaking
