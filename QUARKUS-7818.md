# QUARKUS-7818 - Introduce basic Semeru AOT support

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7818

Upstream issue: https://github.com/quarkusio/quarkus/issues/51978

Upstream PR: https://github.com/quarkusio/quarkus/pull/53339

This feature extends the existing `quarkus.package.jar.aot.enabled=true` workflow to support IBM Semeru Runtimes.
When the build JVM is IBM Semeru, Quarkus automatically generates an OpenJ9 Shared Classes Cache (SCC) instead of a
Leyden AOT file or an AppCDS archive. The SCC stores parsed class metadata, AOT-compiled native startup methods, and 
JIT profiling hints, enabling significantly faster startup times on subsequent runs.

The feature is activated with the same single property used for Leyden and AppCDS — Quarkus detects the JVM at build
time and selects the appropriate strategy automatically. Container image integration is explicitly out of scope for 
this iteration: the automated AOT-enhanced container image workflow available for Leyden is not yet extended to SCC.

Auto-detection logic (`java.runtime.name` contains `semeru` → SCC; OpenJDK 25+ → Leyden AOT; older OpenJDK → AppCDS)
is implemented in `JvmStartupOptimizerArchiveBuildStep`. The type can also be forced via
`quarkus.package.jar.aot.type=SCC`. Leverage `@EnabledOnSemeru` / `@DisabledOnSemeru` annotations (from Quarkus 
upstream) to guard SCC-specific tests.

## Scope of the testing

In scope:

* The SCC directory (`app-scc/`) is generated when building with `quarkus.package.jar.aot.enabled=true` on a Semeru JVM.
* An application started with `-Xshareclasses:name=quarkus-app,cacheDir=app-scc,readonly -jar quarkus-run.jar` serves
  requests correctly, confirming that the cache does not corrupt the runtime.
* The `auto` type selects SCC on Semeru and does not select SCC on a non-Semeru JVM.
* Explicitly setting `quarkus.package.jar.aot.type=SCC` forces SCC generation regardless of the auto-detection result.
* Setting `quarkus.package.jar.aot.phase=integration-tests` populates the cache during `@QuarkusIntegrationTest`
  execution (the recommended training path).

Out of scope:

* Automated AOT-enhanced container image builds with SCC — not implemented in this iteration.
* Startup time benchmarking — performance is the motivation for the feature, not a correctness criterion.
* Non-x86_64 architectures — Semeru CI infrastructure is currently x86_64 only.
* OpenShift — the SCC directory is a local filesystem artifact produced at build time; Semeru is not the OpenShift
  runtime JVM.

Modes: JVM only (SCC is a JVM-only feature; native mode is not applicable).

## Extension interactions and integration points

The SCC feature sits at the build/packaging layer and has no runtime interaction with Quarkus extensions:
any extension that works in JVM mode with `aot-jar` packaging is unaffected.
The only integration point is the `quarkus-container-image-*` family — explicitly excluded above because Quarkus does
not yet wire the SCC output into the container image build step.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed the upstream issue and pull request.
* Reviewed `JvmStartupOptimizerArchiveBuildStep`, `JvmStartupOptimizerArchiveType`, `PackageConfig.AotConfig`,
  `EnabledOnSemeru`, and `EnabledOnSemeruCondition`.
* Reviewed the upstream integration test `JarRunnerIT#testThatSccFileUsable`.
* Reviewed documentation on [AOT caching](https://quarkus.io/guides/aot) and 
[Faster Startup on IBM Semeru with OpenJ9 Shared Classes Cache Blog](https://quarkus.io/blog/semeru-scc/).
* Reviewed existing QE coverage in `packaging/jar`, `packaging/tree-shake`, and 
[QUARKUS-6752](https://redhat.atlassian.net/browse/QUARKUS-6752).

## Existing test coverage

Upstream integration test `JarRunnerIT#testThatSccFileUsable` (guarded by `@EnabledOnSemeru`) builds a minimal
`scc` project with `quarkus.package.jar.aot.enabled=true`, verifies the build log contains `SCC cache`, and then
starts the packaged JAR with `-Xshareclasses:name=quarkus-app,cacheDir=app-scc,readonly` to assert that a `/hello`
endpoint responds correctly.

The QE test suite has no SCC coverage. The `packaging/jar` and `packaging/tree-shake` modules exercise JAR packaging
without AOT enabled and have no SCC-specific tests. In `external-applications`, the `AoTQuickstartIT` checks the AoT 
case, but there is no SCC or Semuru specific validation.

## Planned coverage

A new `packaging/semeru-scc` module will be added, following the structure of `packaging/tree-shake`.
The application under test will use `quarkus-rest`, `quarkus-rest-jackson`, `quarkus-hibernate-orm`, and
`quarkus-jdbc-h2`. Hibernate ORM should cause a significant class-loading heavy work that the SCC must cache
correctly. All scenarios below exercise this full application, so any extension-specific startup breakage under SCC
will surface in scenario 1.

1. **SCC auto-detection on Semeru** (`@EnabledOnSemeru`) — build with `quarkus.package.jar.aot.enabled=true` and no
   explicit type; assert that `app-scc/` is created and the application starts and serves responses when launched with
   `-Xshareclasses:name=quarkus-app,cacheDir=app-scc,readonly`.

2. **Explicit SCC type** (`@EnabledOnSemeru`) — build with `quarkus.package.jar.aot.type=SCC`; assert the same
   outcome as above, confirming that the explicit override works independently of the auto-detection path.

3. **Integration-test training phase** (`@EnabledOnSemeru`) — build with
   `quarkus.package.jar.aot.phase=integration-tests` and run `@QuarkusIntegrationTest`; assert that the training run
   completes without error and that a subsequent cold start with
   `-Xshareclasses:name=quarkus-app,cacheDir=app-scc,readonly` still serves requests correctly, confirming that
   incremental training does not corrupt the cache.

4. **Non-Semeru guard** (`@DisabledOnSemeru`) — on non-Semeru JVMs, verify that
   `quarkus.package.jar.aot.enabled=true` with `type=AUTO` does not produce an `app-scc/` directory and that the
   application starts and serves requests normally, confirming that auto-detection does not misfire.

5. **Missing cache graceful handling** (`@EnabledOnSemeru`) — after building with SCC enabled, delete the `app-scc/`
   directory; start the application with `-Xshareclasses:name=quarkus-app,cacheDir=app-scc,readonly`; assert that
   the application still starts and serves requests correctly, verifying that the Quarkus packaging does not suppress
   the OpenJ9 `readonly` silent-fallback behaviour when the cache is absent.

!!! warning

    Upstream has `@EnabledOnSemeru` and `@DisabledOnSemeru`. The Quarkus Test Framework only has `@DisabledOnSemeruJdk`. 
    Analyze if we can use the upstream annotation to control test execution or implement parity in the Quarkus Test 
    Framework with a `@EnabledOnSemeruJdk`.

## Impact on test suites and testing automation

A new `packaging/semeru-scc` module will be added. Each scenario uses an independent build; the `app-scc/` directory
is produced fresh per test, ensuring no cross-scenario contamination.
Non-Semeru jobs will skip the `@EnabledOnSemeru`-guarded tests and only run the non-Semeru guard (scenario 4).

A Semeru 25 GitHub Actions job will be added so that SCC coverage runs in the upstream-aligned CI environment in
addition to the existing Semeru baremetal pipeline introduced by QUARKUS-6752.

## Impact on resources

The added tests should have **big impact** on resources:

* Estimated execution time increase: 2 to 3 hours with a new Semeru job (executed in parallel with other runners, may 
not increase the total build time, if enough runners are available)
* Tests will be executed on baremetal in JVM mode only (Semeru x86_64).
* No impact on native jobs or OpenShift jobs.

## Contacts

* Tester: Roberto Cortez <radcortez@ibm.com>

## References

* https://quarkus.io/guides/aot
* https://quarkus.io/blog/semeru-scc/
* https://eclipse.dev/openj9/docs/shrc/
* https://github.com/quarkusio/quarkus/pull/53339
* https://github.com/quarkusio/quarkus/issues/51978
* [QUARKUS-6752](QUARKUS-6752.md)
