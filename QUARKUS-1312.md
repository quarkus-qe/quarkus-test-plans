# QUARKUS-1312 Integrate performance testing into the QE Acceptance pipeline

This document describes a plan of automation of testing performance of RHBQ with promoted productized builds.

## Scope of the testing

### Impact on test suites and testing automation
* A job that triggers Quarkus QE start-stop performance metrics in perflab should be implemented and added to 
  acceptance pipeline.
* A job triggering TechEmpower testing in perflab should be implemented and ran with candidate releases. Thus this job
  will be added to extended platform pipeline, which is only ran with builds of required quality.

### Impact on resources
* For both the start-stop and TechEmpower trigger job, a minimal possible executor will be used. This is currently a 
  medium machine.
  * Start-stop testing takes minutes to tens of minutes.
  * TechEmpower testing takes tens of minutes to hours.

## Future considerations
* For TechEmpower testing, we should explore using a Horreum dashboard ran in perflab to visualize and compare trends 
  between releases.

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature epic: [QUARKUS-1312 Integrate performance testing into the QE Acceptance pipeline](https://issues.redhat.com/browse/QUARKUS-1312)
