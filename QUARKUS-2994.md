# QUARKUS-2994 JDK 21 Support

This document is a high level test plan for certification of Red Hat build of Quarkus on JDK 21.

JIRA link: https://issues.redhat.com/browse/QUARKUS-2994

## Scope of the testing

### Current test coverage
* Regular CI runs in upstream include full testing for Java 21 (according to the announcement)
* Daily runs of test suite on Java 21 in GitHub Actions
* Checks of code.quarkus in StartStop TS

### Impact on test suites and testing automation
* Temurin JVM 21 and OpenJDK 21 need to be added as JDKs to Quarkus QE Jenkins
* Temurin JVM 21 and OpenJDK 21 have to be added as JVMs to Polarion
* Bare metal jobs in extended platform testing need to be extended to run with both Temurin 21 and OpenJDK 21.
  * This affects JVM mode only.
* OpenShift JVM job needs to use OpenJDK 21 (together with existing 17) instead of OpenJDK 11
  * We also should use S2I build image based on JDK 21 there
* Two quickstart jobs (Linux and Windows) should be added/repurposed from 11 in general or extended pipeline
* Add a job for running on ARM

### Impact on resources
* Should not change (or be minimal), since for affected jobs we should drop testing on Java 11, so consumed resource will stay the same.
* In general support for JDK depends on number of used jobs. For basic product support, each run requires the following resources on OpenStack:
  *  Baremetal 3 combinations (podman, docker, fips) * 8 scenarios * `large`(8192 RAM + 4 vCPU) node
  *  OpensShift two combinations (eus, stable) * `medium`(4096 RAM + 4 vCPU) node.
    * Each combination includes ~180 tests (running together for 40 minutes), each creates and deletes a dedicated OpenShift namespace(6 m1.large OpenStack machines)  
  * Quickstarts on RHEL: `large` jenkins node (see above)
  * Quickstarts on Windows: `medium` jenkins node and `m5zn.metal` EC2 instance
  * ARM job: 8 scenarios *  large node + OCP cluster on AWS (6x xlarge Graviton, see also "Resource consumption on AWS" link in References)
  
## Future considerations
* Since updating JDKs require additional effort on both ours and infra team side, we may have to come up with setting up the JDK ourselves
  * If snapshot-local setup is used, testing snapshots have to be respun every time a release is made(but we do it every two weeks anyway)
  * If we install JDK as part of the testing job, this will make using matrix jobs very inconvenient

### Impact on documentation
* Documentation should mention JDK 17 and 21, not JDK 11
* Documentation should mention, that S2I works with both JDK 17 and JDK 21
* Update testing matrices in our Signpost 

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@redhat.com>

## References
* Feature epic: [QUARKUS-2994 JDK 21 Support](https://issues.redhat.com/browse/QUARKUS-2994)
* Temurin support TP: [QUARKUS-1936](QUARKUS-1936.md)
* Upstream support announcement: https://quarkus.io/blog/quarkus-3-5-0-released/
* Resource consumption on AWS: https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-3004.md#impact-on-resources
