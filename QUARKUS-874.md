# QUARKUS-874 Production support for Windows

JIRA link: https://issues.redhat.com/browse/QUARKUS-874

The goal is to provide production support for JVM mode on Windows and verify that deployment from Windows to the OpenShift works.

## Existing coverage
We already have Windows as part of the Quarkus QE Test Suite GitHub CI, where we run tests that do not require Linux containers.
As part of the extended platform trigger, we already run StartStop Special Chars tests, as well as the Quarkus QE Test Suite
in OpenShift (http-minimum module) using the OpenShift extension. The Quarkus Quickstarts modules that doesn't require
Linux containers are run as well.

## Scope of the testing
In addition to what we already test, our primary focus needs to be on bare-metal JVM tests and ensure we run 
same tests as we run for the Red Hat Enterprise Linux operating system, except for extensions that require native 
libraries (like the Apache Kafka Streams).

### Selected Windows Platforms
- Windows Server 2019 for GitHub CI and Jenkins Start-Stop Test Suite and Test Suite JVM in OpenShift
- Windows Server 2022 for Jenkins jobs for weekly Test Suite runs and jobs that are part of RHBQ extended trigger:
  - Quarkus Quickstarts in JVM mode on bare metal
  - Quarkus QE Test Suite in JVM on bare metal

### Selected Test Suites
 - Start-Stop Test Suite
   - In Github CI - for Pull Requests
   - New Jenkins Jobs
 - Beefy Test Suite
   - In Github CI - for Pull Requests and Daily runs
   - New Jenkins Jobs
 - Combinations of Quarkus extensions Test Suite
   - In Github CI - for Pull Requests and Daily runs
 - Quarkus QE Test Suite
   - In Github CI - for Pull Requests and Daily runs
   - New weekly Jenkins Job running against Quarkus main

## Impact on Jenkins
 - New Jobs definitions
 - Rename existing jobs to differentiate them for those running on RHEL and on Windows platforms
 - New Jenkins AWS runners as our tests rely mostly on the TestContainers, we need Linux containers and hardware virtualization respectively, which is not possible in the RHOS-D.

## Impact on resources
The major impact is on the PSI and Jenkins capacity:
 - Windows instances will be created from RHEL instances, therefore we need at least one RHEL instance of medium flavor for each job.
 - Windows instances that requires Docker will be run in AWS, therefore only impact to the current capacity is above-mentioned RHEL instance. 
 - Windows instances that do not require Docker will be executed next to the RHEL instances, but they are mostly lightweight short-running jobs
 - Overall test execution for RHEL and Windows jobs will be affected (doubled in the worse case)
 - More jobs pulling artifacts from Artifactory instance
 - We may need to use more resources for Artifactory
 - Weekly Test Suite run on AWS will require around 9.88 h per week. The execution time will be longer as our Test Suite will cover more topics over time.

## Contacts
* Tester: Jose Carvajal <jcarvaja@redhat.com>
* Tester: Michal Vavřík <mvavrik@redhat.com>