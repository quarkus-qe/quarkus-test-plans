# QUARKUS-1391 Make podman a first class container technology for Quarkus Development experience

JIRA link: https://issues.redhat.com/browse/QUARKUS-1391

The goal is to make sure, that Podman can be used instead of Docker for Quarkus in dev mode. 

## Scope of the testing
The selected test suites will be executed on the selected platforms with podman as a container engine to ensure functionality is fully accomplished. Moreover, these test suites might need adjustments for the correct execution.

In order to accommodate this in our current test suites, we need to add new jobs in Jenkins. 

### Selected Platforms
- RHEL
- Mac OS (due to the lack of Mac OS on servers, we will not automate checks on this platform)
- Windows Server 2022

### Selected Test Suites
- Quarkus Test suite
  - Jenkins jobs
  - GitHub CI jobs
- Quickstarts

## Impact on Jenkins
 - New jobs definitions
 - New image builder jobs
 - New images

## Impact on resources
The major impact is on the PSI and Jenkins capacity:
- New instances for jobs on Jenkins
- New baremetal Windows instances on AWS cloud

## Contacts
* Tester: Fedor Dudinski <fdudinsk@redhat.com>
