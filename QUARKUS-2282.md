# QUARKUS-2282 Adjustable dependency check

Jira: https://issues.redhat.com/browse/QUARKUS-2282

This feature is about externalization of test verifying productization of build and runtime dependencies of supported
extension that currently lives in [quickstarts-ts-jvm-acceptance-quarkus-bom](https://gitlab.cee.redhat.com/quarkus-qe/jenkins-jobs/-/blob/main/jobs/rhbq/rhel8_jdk11_quickstarts_ts_jvm_acceptance.groovy)
job to meet the following criteria:

1. The implementation of the check itself should be separated from its configuration. Given that each product build may 
   have its own allowlist of dependencies that are expected to be not productized, this list should not be hardcoded in 
   the script.
2. The script should be hosted in a location accessible by both the prod and QE teams/processes, so that both teams use 
   the same implementation of the check.

## Scope of the testing

### Impact on test suites and testing automation
* The shell script from [quickstarts-ts-jvm-acceptance-quarkus-bom](https://gitlab.cee.redhat.com/quarkus-qe/jenkins-jobs/-/blob/main/jobs/rhbq/rhel8_jdk11_quickstarts_ts_jvm_acceptance.groovy)
  job will be extracted into a stand-alone shell script that will be executable in stand-alone. The shell script will:
  * download the zipped Maven repository with Quarkus platform to test;
  * build the supported set of quickstarts with the platform from the Maven repository;
  * check the dependency list of quickstarts for unproductized dependencies and compare them with allowlist;
  * check the local Maven repository for unproductized dependencies and compare them with allowlist.
* The shell script will be updates to take the file with the list of dependencies as parameter.
* The shell script and dependency list will be placed in well known location in [quarkus-qe/jenkins-jobs](https://gitlab.cee.redhat.com/quarkus-qe/jenkins-jobs)
  repository.
* The job will be updated to fetch and use the shell script from the well known location.

### Impact on resources
None.

## Contacts
* Assignee: Michal Jurč <mjurc@redhat.com>

## References
* Feature issue: [QUARKUS-2282 Adjustable dependency check](https://issues.redhat.com/browse/QUARKUS-2282)
