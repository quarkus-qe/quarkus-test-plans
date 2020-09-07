# QUARKUS-270 - Testing and verifying OpenShift Serverless Serving with Quarkus

JIRA link: https://issues.redhat.com/browse/QUARKUS-270

Key point for this release is that it should be just as easy to deploy a serverless application as it is to deploy a normal OpenShift container when using Quarkus.

## Scope of the testing
Following actions were taken to ensure test coverage and automation for serverless applications:
 - Define the scope of the testing
 - Analyze the possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
 - Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources
 - Check a need for involvement of other product/component teams

### Impact on testsuites and testing automation:
 - OpenShift TS framework needs to be enhanced to support OpenShift Serverless Serving
   - knative client, different routes, different deployment-target
   - new maven profile to enable OpenShift Serverless Serving tests on request (OpenShift 4.5+)
 - New test coverage for Serverless/Knative Serving scenario needs to be added into OpenShift TS
 - Ensure this coverage works both in JVM and NATIVE mode
 - Relevant jobs for testing with OpenShift TS need to be updated
 - Automation for provisioning of OpenShift cluster needs to be enhanced to:
   - install OpenShift Serverless Operator
   - install Knative Serving
   - make Serverless installation optional - e.g. not available in nightly OpenShift builds

### Impact on resources:
 - More services will be running on OpenShift cluster, impact on CPU/memory should be minimal
 - QUARKUS-270 covers both JVM and NATIVE mode, Native image imposes multiple times prolonged execution time and increased memory requirements for the build

Our current testing capacity is sufficient to cover this testing.

If multiple streams get introduced we need to have increased capacity in the lab.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use
 - Check the impact on memory footprint and boot time
 
Activity in this regard already lead to discussions - e.g. identified need for zero-config solution for OpenShift Serverless Serving

Memory footprint and boot time were checked using OpenShift web console.

## Automated test development
This step should ensure:
 - Focus on high-level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage

This feature request is specifically targeting OpenShift platform, test coverage will be added into OpenShift TS. Same experience as with vanilla OpenShift needs to be achieved with OpenShift Serverless Serving. If that's not the case follow-up actions need to be identified.

## Advanced topics for test development
The following advanced topics are listed for future consideration. There should be dedicated feature requirements and/or enhancements for these specific areas.
 - Security testing
 - Recovery/Failover testing
 - Performance/Scalability testing
 - Backward compatibility testing