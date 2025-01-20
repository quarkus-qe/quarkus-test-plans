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
 - For each feature / Quarkus extension, assess existing and consider implementing new integration tests for areas (when applicable):
   - Logging
   - Tracing
   - Metrics
   - Security
   - OpenAPI
   - Data sources
   - Qute
 - Analyze possible impact on Quarkus itself - e.g performance, compatibility
 - Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
 - Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources
 - Check a need for involvement of other product/component teams

## Getting familiar with the feature
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use
 - Check impact on disk + memory footprint and boot time
 - Check the community status of the extension/feature, preview and experimental ones should be considered carefully
 
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

# Notes for community status of the extensions/features
The ideal goal is to discuss the JIRA with PM and DEV and decide on the next steps based on your input.

## Preview status
If the extension/feature will be in a preview state forever, the request for product support should be reconsidered.
If the preview state is planned to be removed soon, a technology preview support level on the product side should be proposed.
Full support should be considered once the community status moves to stable.

## Experimental status
Experimental community status should be a showstopper for full or technology preview product support.