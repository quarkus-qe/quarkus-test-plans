# QUARKUS-8888 - Implement SBOM contract to integrate with RHACS scanner

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8888

Documentation: https://quarkus.io/guides/cyclonedx

Codebase: https://github.com/quarkusio/quarkus/tree/main/extensions/cyclonedx

Upstream PRs:
 * https://github.com/quarkusio/quarkus/pull/56489
 * https://github.com/quarkusio/quarkus/pull/56503
 * https://github.com/quarkusio/quarkus/pull/56494

Implementing the SBOM contract to integrate with the RHACS scanner solution depends on the existing CycloneDX SBOM extension, with
addition of several features to the extension itself, platform generator metadata, and platform generator tooling.

Full support of the CycloneDX SBOM extension is part of this RFE.

This test plan focuses on Quarkus 3.40 LTS. There is a backport of the integration into Quarkus 3.33 LTS which won't bring
all the available features in the CycloneDX SBOM extension (e.g. SBOM REST endpoint).

## CycloneDX SBOM extension basics

The `quarkus-cyclonedx` extension generates a Software Bill of Materials (SBOM) in CycloneDX format for a Quarkus
application. It matters for supply-chain security and compliance: the SBOM feeds vulnerability scanners (Grype, syft)
and compliance tooling. The extension covers several distinct capabilities that must all be validated:

1. **Distribution SBOM** — automatically emitted next to the build output (`<executable-name>-cyclonedx.<format>`) once
   `io.quarkus:quarkus-cyclonedx` is a dependency; describes the actual build artifact.
2. **Embedded SBOM** — SBOM packaged as a classpath resource (`META-INF/sbom/dependency.cdx.json`).
3. **SBOM REST endpoint** — exposes the embedded SBOM at `/.well-known/sbom` (needs `quarkus-vertx-http`).

Key behaviours that differ by packaging type: `evidence.occurrences.location` is present for fast-jar / mutable-jar,
absent for uber-jar and native; build-time deps always carry CycloneDX scope `excluded` / `development`; the serial
number is reproducible when `project.build.outputTimestamp` is set.

## RHACS scanner integration basics

The `quarkus-cyclonedx` extension will be enhanced to add Common Platform Enumeration (CPE) into SBOMs.
This functionality will be enabled by the platform metadata enhancement available just for the product platforms.
RHACS scanner will be able to identify the product that the application is based on and connect the relevant VEX details. 

## Scope of the testing

In scope:
- Distribution SBOM generation for **fast-jar (default), uber-jar, mutable-jar, native-image** packaging types.
- Config matrix: `format` (json / xml / all), `schema-version`, `pretty-print`, `runtime-only`, `libraries-only`,
  `include-license-text`, `include-quarkus-component-scope`.
- Embedded SBOM (`quarkus.cyclonedx.embedded.*`) including GZIP compression on/off and custom `resource-name`.
- SBOM REST endpoint (`quarkus.cyclonedx.endpoint.*`) on the main HTTP port and on the management interface (9000),
  content type `application/vnd.cyclonedx+json`.
- SBOM correctness: main component coordinates/type/purl, presence of runtime + build-time components, scopes,
  component locations, valid CycloneDX schema.
- Consuming the SBOM with a real scanner (syft / Grype) to prove the output is usable, not just well-formed.
- Documentation review of the guide against actual behaviour and config defaults.
- Common Platform Enumeration (CPE) availability and correctness for product platforms.
- Marete checks for platform support metadata and artifacts productization.
- Marete test to ensure CPE metadata availability.

Out of scope:
- Dependency SBOMs produced by `mvn quarkus:dependency-sbom` goal
- Gradle SBOM publishing (uses the upstream CycloneDX Gradle plugin, not Quarkus code) — smoke check only.
- `SbomContributionBuildItem` SPI for third-party extension authors — validated only indirectly through core extensions.
- `quarkus.package.jar.tree-shake` option to eliminate unused classes from JARs, it is experimental feature that is disabled by default.
- Deep validation of CycloneDX spec internals, we rely on the CycloneDX library bake time, we primarily validate Quarkus wiring.

Modes: JVM and Native

Platforms:
 * RHEL and Windows
 * OpenShift coverage will be limited to 1-2 scenarios
   * Embedded SBOM mode makes no difference between bare metal and containerized environments  

## Extension interactions and integration points

- **quarkus-vertx-http** — mandatory for the SBOM endpoint; verify graceful behaviour when endpoint is enabled without it.
- **Security (OIDC / basic auth)** — the endpoint leaks component inventory; verify it can be secured and is not exposed
  by default (`endpoint.enabled=false`). This is the most important integration concern for RH/IBM product use.
- **Management interface** — endpoint routing when `quarkus.management.enabled=true`.
- **Product and non-product extensions** — multi-extension app to confirm all runtime components appear in
  the SBOM with correct scopes and locations.
- **Reproducible builds** — `project.build.outputTimestamp` must yield a stable `urn:uuid:` serial number.

## Getting familiar with the feature

- Reviewed the `cyclonedx` guide and the extension modules.
- Studied the upstream ITs `CycloneDxIT`, `CycloneDxNativeIT`, `CycloneDxTestUtils` to learn the expected component
  assertions per packaging type.
- Focus on exploratory testing across the four SBOM capabilities and the config matrix.

## Existing test coverage

Upstream integration tests:
- `integration-tests/maven/.../CycloneDxIT.java` — asserts distribution SBOM for **fast-jar, uber-jar, mutable-jar** and
  the **embedded SBOM in fast-jar**. Checks main component coordinates/type/purl, component presence, scopes
  (`runtime` / `development`) and locations (`lib/main/`, `lib/deployment/`).
- `integration-tests/maven/.../CycloneDxNativeIT.java` — native distribution SBOM (generic file main component,
  no locations) plus round-trip verification of the embedded SBOM with **syft**.
- `TreeShakeSbomIT.java` — SBOM interaction with the tree-shaking feature (`pedigree` / removed classes).
- `ManagementInterfaceTestCase.java` - asserts **REST endpoint** and its **security/management-interface** behaviour

Gaps upstream: no coverage of the `format=xml`/`all`, `schema-version`, `pretty-print`, `runtime-only`,
`libraries-only`, `include-license-text` config options; no dependency-SBOM goal coverage across bootstrap modes;
no Grype vulnerability-scan round trip; no OpenShift/container coverage..

## Planned coverage

A new `sbom/cyclonedx` module with a multi-extension app (REST + Hibernate ORM + Qute + Quarkus Amazon Services) will provide
coverage for the topics mentioned in the `Scope of the testing` section.

Embedded SBOM mode will receive extra focus, as that's the intended mode for usage with the RHACS scanner.

## Impact on test suites and testing automation

 - New `sbom/cyclonedx` module; existing modules are unaffected.
 - New `sbom/cyclonedx` module will be part of `root-modules` profile.
 - Syft/Grype must be available (or containerised) on the agents running the scanner round trip.
 - Native tests can reuse the syft-based verification pattern from upstream. 

## Impact on resources

Executed on bare metal, JVM + Native. Extra cost comes from building the same app in four packaging types, one native
build. This gets multiplied by the number of tested combinations of the configuration properties, only two scenarios are planned
for native mode to save time. Estimated added execution time ~15-20 minutes (native build dominates).

## Contacts

* Tester: Rostislav Svoboda <rsvoboda@ibm.com>

## References

- https://quarkus.io/guides/cyclonedx
- https://github.com/quarkusio/quarkus/tree/main/extensions/cyclonedx
- https://github.com/quarkusio/quarkus/blob/main/integration-tests/maven/src/test/java/io/quarkus/maven/it/CycloneDxIT.java
- https://github.com/quarkusio/quarkus/blob/main/integration-tests/maven/src/test/java/io/quarkus/maven/it/CycloneDxNativeIT.java
