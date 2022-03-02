# quarkus-test-plans
Test Plans for tested features of Quarkus product

## Naming convention
Use file name based on RFE id, for example QUARKUS-123.md

# Topics to be considered in test plan

## Links to the resources
 - Provide links to the GH / JIRA issue
 - Provide links to the Chatroom / Forum design discussions
 - Provide links to other related test plans if applicable 

## Scope of the testing
 - Define scope of the testing
 - Analyze possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
 - Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources
 - Check a need for involvement of other product/component teams

## Getting familiar with the feature
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use
 - Check impact on disk + memory footprint and boot time
 
## Automated test development
 - Focus on high level scenarios with multiple components and features involved
 - OpenShift should be the primary target platform for product to ensure good interoperability
 - Ensure platform specific features or components have appropriate test coverage

## Advanced topics for test development
 - Security testing
 - Recovery/Failover testing
 - Performance testing
 - Backward compatibility testing

These advanced topics are listed to be considered, there is no guarantee to have sufficient manpower and time to focus on them.