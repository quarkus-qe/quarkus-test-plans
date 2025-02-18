# QUARKUS-4573 - Switch to UBI9 as base runtime container

JIRA: https://issues.redhat.com/browse/QUARKUS-4573

Updated docs:
- https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.19#ubi-9
- https://quarkus.io/version/main/guides/quarkus-runtime-base-image

Upstream issues: 
- https://github.com/quarkusio/quarkus/issues/45968
- https://github.com/quarkusio/quarkus/issues/46334

Upstream PRs:
- https://github.com/quarkusio/quarkus-images/pull/283
- https://github.com/quarkusio/quarkus-images/pull/283
- https://github.com/quarkusio/quarkus-images/pull/288
- https://github.com/quarkusio/quarkus/pull/46231
- https://github.com/quarkusio/quarkus/pull/45973
- https://github.com/quarkusio/quarkus/pull/46338

Quarkus switched default Universal Base Image (UBI) used in its images from 8.x to 9.x in order to support customers using UBI 9.
It is important that Quarkus remains compatible with UBI8 images because there are still customers requiring the support.
The switch to UBI must happen on all the levels, it is not possible to combine UBI 8 based Docker files with UBI 9 based native builder image.

## Scope of the testing
Switch all the existing tests to UBI 9 with strong preference for the OpenJDK 21 version, so that we test:

- `registry.access.redhat.com/ubi9-minimal:9.5`
- `registry.access.redhat.com/ubi9/openjdk-21:latest`
- `registry.access.redhat.com/ubi9/openjdk-17:latest`
- `ubi9-quarkus-graalvmce-builder-image:jdk-21`
- `ubi9-quarkus-graalvmce-s2i:jdk-21`
- `ubi9-quarkus-mandrel-builder-image:jdk-21`
- `ubi9-quarkus-micro-image:2.0`
- `ubi9-quarkus-native-binary-s2i:2.0`
- `registry.access.redhat.com/ubi9/openjdk-21-runtime`
- `registry.access.redhat.com/ubi9/openjdk-17-runtime`

And compatibility will be assured by running tests with images that were default previously:

- `registry.access.redhat.com/ubi8/ubi-minimal:8.9`
- `registry.access.redhat.com/ubi8/openjdk-17:latest`
- `ubi-quarkus-graalvmce-builder-image:jdk-21`
- `ubi-quarkus-graalvmce-s2i:jdk-21`
- `ubi-quarkus-mandrel-builder-image:jdk-21`
- `quarkus-micro-image:2.0`
- `ubi-quarkus-native-binary-s2i:2.0`
- `registry.access.redhat.com/ubi8/openjdk-21-runtime`
- `registry.access.redhat.com/ubi8/openjdk-17-runtime`

Images based on UBI 8/9 will be tested with Docker files using the same UBI version (8 or 9).

Proposed new _compatibility_ test coverage should be:

- every OpenShift job running Quarkus QE Test Suite with Quarkus 3.19+ will test:
  - root modules (important because there is a Docker/Podman build, external applications, super-size applications)
  - messaging modules
- RHBQ builds in the general trigger pipeline will have baremetal jobs enhanced to run:
  - Docker / Podman build submodules in JVM
  - root modules and monitoring modules in native
- Quarkus QE Test Suite daily builds in GitHub will test:
  - Docker / Podman build submodules in JVM
  - all native mode tests run with the `ubi-quarkus-mandrel-builder-image:jdk-21` image

Selection of messaging and monitoring modules is a random choice, I don't see compelling reasons to test any particular module apart from the root modules.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
Quarkus QE run all the test with UBI 8 images mentioned above.
This has changed when Quarkus changed defaults, because we do not specify all the images explicitly, for 
example `ubi9-quarkus-micro-image:2.0` comes from default value in the `quarkus.jib.base-native-image` configuration property.
Quarkus main project (upstream) is using defaults as well, the images are well tested.
Unlike us, the upstream project uses concrete image versions, not the `latest` tag, it helps them to manage expectations and makes branching easier.
Some image versions, like `openjdk-21-runtime` vs `openjdk-17-runtime` are determined dynamically based on Java versions with which Java sources were compiled.
I don't see where that would work for UBI 8 though, this need to be verified.

### Impact on test suites and testing automation
Quarkus QE Test Framework will be enhanced to support switching between UBI 8 and 9 for Docker files.
Now new tests will be added, we will extend test coverage by running new matrix job combinations.

### Impact on resources
Test execution time will increase roughly by:
- Quarkus QE Test Suite GitHub Daily JVM build: 4 minutes (reference point: daily build #1505)
- Quarkus QE Test Suite GitHub Daily native build: 11 hours (reference point: daily build #1505)
- Quarkus QE Test Suite OpenShift native build: 3 hours (reference point: openshift-weekly-ts-native-ocp-4-stable #14)
- Quarkus QE Test Suite OpenShift native build: 2 hours and 20 minutes (reference point: openshift-weekly-ts-jvm-ocp-4-stable #15)
- RHBQ JVM build: 4 minutes (shouldn't significantly differ from respective GitHub reference above)
- RHBQ native build: 4 hours (reference point: execution time from the runs before the UBI 8 switch)

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [Quarkus QE tracker](https://issues.redhat.com/browse/QQE-1340)
- [QUARKUS-4571 - UBI9 and RHEL9 support](https://issues.redhat.com/browse/QUARKUS-4571)
- [QUARKUS-5638 - Document scope of support for RHBQ when run in FIPS-enabled environment](https://issues.redhat.com/browse/QUARKUS-5638)