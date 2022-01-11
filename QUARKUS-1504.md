# QUARKUS-1504 - Update tools provided Dockerfiles to use RHEL 8.5

JIRA link: https://issues.redhat.com/browse/QUARKUS-1504

Update tools provided Dockerfiles to use RHEL 8.5 based UBI image. The follow-up task is to ensure QE lab is using RHEL 8.5.

## Scope of the testing
- Make sure `ubi8/ubi-minimal:8.5` is used in the newly generated project
- Make sure `ubi8/ubi-minimal:8.5` is used for testing in quarkus-test-suite
- Execution of container image build succeeds
- Execution of generated container image succeeds
- Temporary Jenkins job to check content of `/etc/redhat-release` on RHEL machines

### Impact on testsuites and testing automation:
- quarkus-test-framework needs to be updated to use `ubi8/ubi-minimal:8.5`
- quarkus-test-suite execution with updated quarkus-test-framework needs to pass

### Impact on resources:
- No additional requirements for resources in lab
- One temporary Jenkins job to ensure RHEL 8.5 is used in QE lab

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the features

## Automated test development
This step should ensure:
- quarkus-test-framework update
- no additional test development

## Advanced topics for test development
N/A