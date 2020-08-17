# QUARKUS-272 - Quarkus Native support via Mandrel

JIRA link: https://issues.redhat.com/browse/QUARKUS-272
GitHub link: https://github.com/quarkusio/quarkus/issues/10367

For native mode similar test coverage as for JVM mode needs to be ensured.

## Scope of the testing
Following actions were taken to ensure test coverage and automation for native image:
 - Define the scope of the testing
 - Analyze the possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
 - Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources
 - Check a need for involvement of other product/component teams

### Impact on testsuites and testing automation:
 - StartStop TS needs to be enhanced and to switch to Mandrel image
 - OpenShift TS needs to support native image deployments and testing
 - New jobs for native testing with OpenShift TS need to be created, same for Polarion
 - OpenShift 3.11 based testing should cover a subset of native image testing
 - Jobs for native testing with Quickstarts need to be created, same for Polarion
 - Tests for code.quarkus should ensure native mode works in generated projects.
 - Workshop tests will remain focused on JVM mode as the workshop is designed that way.
 - Beefy-scenarios testsuite should have a job for native mode, same for Polarion

### Impact on resources:
 - Native image imposes multiple times prolonged execution time
 - Native image imposes increased memory requirements (6 GB+ of RAM)
 - Investigation of issues in native application is more complex and time-consuming

Our Current testing capacity should be sufficient to cover this testing.

If multiple streams get introduced we need to have increased capacity in the lab.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use
 - Check the impact on disk + memory footprint and boot time
 
Activity in this regard already leads to discussions - e.g. just community support for S2I on OpenShift with native image.

Disk size and memory footprint are checked on the community side and thus creating test coverage for this on the product side is not a hard requirement.

## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage

Native image test development translates for us to ensure equivalent test coverage we have JVM mode. Details in `Impact on testsuites and testing automation` section above.

Native image specific test development will ensure product specific aspects - e.g. product specific docker/podman image for building native binary.

## Advanced topics for test development
The following advanced topics are listed for future consideration. There should be dedicated feature requirements and/or enhancements for these specific areas.
 - Security testing
 - Recovery/Failover testing
 - Performance testing
 - Backward compatibility testing