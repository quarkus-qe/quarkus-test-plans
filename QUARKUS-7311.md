# QUARKUS-7311 Testing on RHEL 9

JIRA: https://issues.redhat.com/browse/QUARKUS-7311

Testing on RHEL 9

## Scope of testing

The [QUARKUS-7311](https://issues.redhat.com/browse/QUARKUS-7311) is QE task which is part of [QUARKUS-4571](https://issues.redhat.com/browse/QUARKUS-4571).
Testing will target RHEL 9 for RHBQ and IEBQ 3.33 (and future releases).
The transition from RHEL 8 to RHEL 9 will be conducted in two phases:
* Phase 1: Migration of the acceptance and general testing pipelines.
* Phase 2: Migration of the extended platform testing pipeline.

Phase 1 will focus on validating RHEL 9 generated images (check for missing packages etc.) and ensuring their readiness for broader testing.
The timeline for Phase 2 will be determined by the results of Phase 1.
If few or no issues are identified, Phase 2 is expected to proceed without risk of delay.
Should significant issues arise, they and their associated risks will be communicated.

To fully test the productized build of Quarkus 3.33, all bare metal native jobs must run on RHEL 9.
It is because the new native builder will not be supported on RHEL 8, as its base image is now UBI9 based.
Attempting to run the native builder on a RHEL 8 machine will result in failures due to missing `glibc` versions.

The OpenStack VM image creation needs to be enhanced with addition of docker OpenStack VM image.
The Openstack VM image combination should match the RHEL 8 (docker, podman, FIPS).

These changes will affect both `x86-64` and `aarch64` architecture.

### RHEL 8 compatibility

For `x86-64` architecture the extended platform testing pipeline will extend `baremetal-ts-jvm` matrix with RHEL 8 option.
The RHEL 8 option will be executed with one Java version only.

For `aarch64` architecture there will be added the RHEL 8 quickstart job as minimal compatibility check.
The running testsuite on RHEL 8 in JVM will be investigated, if it is feasible.  



## Testing of upstream Quarkus (999-SNAPSHOT)

Daily and weekly tests executed on Jenkins should be migrated to RHEL 9, as future releases will no longer target RHEL 8.
This migration will be handled as a separate task, to be completed toward the end of the 3.33 testing cycle or shortly thereafter.


## Current testing environment
* All jobs are currently executed on RHEL 8.
* There are no jobs which are currently executed on RHEL 9.


## Impact on resources
There will be increase of number of runners, as there still will be check of compatibility in `baremetal-ts-jvm` job.
This check will be for `x86-64` architecture.
For `aarch64` architecture there should be minimal impact in case of number of runner.
This can't be fully described without experimenting with full `aarch64` pipeline. 

Execution times will be slightly increased as there will be few compatibility jobs RHEL 8 jobs.

The impact on storage will be minimal,as there is need to store additional RHEL 9 image.


## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>

## References
* https://issues.redhat.com/browse/QUARKUS-7311
* https://issues.redhat.com/browse/QUARKUS-4571