# QUARKUS-1936 Certify RHBQ on Eclipse Adoptium Builds

This document is a high level test plan for certification of Red Hat build of Quarkus on Temurin JVM.

## Scope of the testing

### Impact on test suites and testing automation
* Temurin JVM 11 and 17 need to be added as JDKs to Quarkus QE Jenkins
  * Temurin JVM 11 and 17 seem to be updated timely on `/qa/tools` mount point, so that will be used for the Elektra
    time frame.
* Temurin JVM 11 and 17 have to be added as JVMs to Polarion
* Bare metal jobs in extended platform testing need to be extended to be ran with both Temurin 11 and 17.
  * This affects JVM mode only.

### Impact on resources
* 1 large executor for bare metal testing for 4 hours on Temurin 11
* 1 large executor for bare metal testing for 4 hours on Temurin 17

## Future considerations
* If Temurin JVM is not updated in timely manner on `/qa/tools` mountpoint, we may have to come up with setting up the
  JDK ourselves
  * If snapshot-local setup is used, testing snapshots have to be respun every time a release is made
  * If we install JDK as part of the testing job, this will make using matrix jobs very inconvenient

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature epic: [QUARKUS-1936 Certify RHBQ on Eclipse Adoptium Builds](https://issues.redhat.com/browse/QUARKUS-1936)
