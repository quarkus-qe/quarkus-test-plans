# QUARKUS-874 Production support for Windows

JIRA link: https://issues.redhat.com/browse/QUARKUS-874

The goal is to ensure some level of testing and identify existing limitations on Quarkus side for Windows.

## Scope of the testing
The selected Test Suites will be executed on the selected Windows Platforms to ensure functionality is fully accomplished. Moreover, these Selected Test Suites might need adjustments for the correct execution on Windows.

In order to accommodate this in our current test suites, we need to rename existing jobs in Jenkins in order to differentiate them for those running on RHEL and on Windows platforms. 

### Selected Windows Platforms
- Windows Server 2019 for GitHub CI and Jenkins environment

NOTE: We will prepare steps to get Windows 10 instance to be able to handle situations when we need manual verification of specific fix for a customer case reproducible just on Windows 10.

### Selected Test Suites
 - Start-Stop Test Suite
   - In Github CI - for Pull Requests
   - New Jenkins Jobs
 - Beefy Test Suite
   - In Github CI - for Pull Requests and Daily runs
   - New Jenkins Jobs
 - Combinations of Quarkus extensions Test Suite
   - In Github CI - for Pull Requests and Daily runs
 - OpenShift Test Suite
   - New Jenkins Jobs on selected modules only (the ones using deployment strategies)

### Platform testing
JVM mode on is the focus for Windows platform. This RFE does not include native executable (Mandrel) or any extension that requires native libraries (like Kafka streams client).

## Impact on Jenkins
 - New Jobs definitions
 - Rename existing jobs to differentiate them for those running on RHEL and on Windows platforms

## Impact on resources
The major impact is on the PSI and Jenkins capacity:
 - Windows instances will be executed next to the RHEL instances 
   - Overall test execution for RHEL and Windows jobs will be affected (doubled in the worse case)
   - Windows virtual machines usually require more resources than RHEL ones, expecting large or xlarge flavor to be used
   - More CPU / memory in the lab would be needed to ensure similar execution time as now.
 - Remote Docker machines instances based on RHEL to run the containers with services like DBs / Keycloak / Kafka / Infinispan for Windows jobs
   - Windows on PSI does not support Linux containers yet, so we want to try to use RHEL instances to run the containers as a workaround
 - More jobs pulling artifacts from Artifactory instance
   - We may need to use more resources for Artifactory

## Contacts
* Tester: Jose Carvajal <jcarvaja@redhat.com>
