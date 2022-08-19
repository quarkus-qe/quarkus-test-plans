# QUARKUS-2230 Introduce OpenShift based CI runs in upstream

## Scope of the testing
* Review the upstream workflow introduced with https://github.com/quarkusio/quarkus/pull/17674.
  * This review will focus on the workflow itself and on test coverage of Quarkus integration surfaces with Kubernetes
    and OpenShift APIs.
  * Once the weak spots in coverage are identified, we will propose enhancements to the workflow.

### Impact on test suites and testing automation
* This feature request is about introduction of new workflow to upstream Quarkus repository that would execute Quarkus
  kubernetes tests on Kubernetes and OpenShift. As such, there is no impact to QE test suites and automation.
* The new workflow should provide actual integration testing for bits and pieces of Quarkus integrated with Kubernetes
  APIs.

### Impact on resources
* None.

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature request: [QUARKUS-2230 Introduce OpenShift based CI runs in upstream](https://issues.redhat.com/browse/QUARKUS-2230)