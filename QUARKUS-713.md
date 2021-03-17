# QUARKUS-713 Tech preview support for the OpenShift Client

Jira: https://issues.redhat.com/browse/QUARKUS-713
GitHub: https://github.com/quarkusio/quarkus/issues/3200

The goal of QUARKUS-713 is to provide tech preview support for the OpenShift client library. This utility will make it 
possible for the users to develop their kubectl plugins or OpenShift operators.

## Scope of the testing

- Define the scope of the testing
- Analyze the possible impact on Quarkus itself - e.g performance, compatibility
- Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
- Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources

### Impact on testsuites and testing automation:
- Upstream smoke test [`OpenShiftClientTest`](https://github.com/quarkusio/quarkus/blob/main/integration-tests/openshift-client/src/test/java/io/quarkus/it/openshift/client/OpenShiftClientTest.java)
  was implemented as part of feature development
- fabric8 client libraries are verified in the [upstream CI](https://github.com/fabric8io/kubernetes-client/actions)
- [quarkus-qe/quarkus-openshift-test-suite](https://github.com/quarkus-qe/quarkus-openshift-test-suite) uses the fabric8 OpenShift client to manage kube objects from test code
- New integration test will be implemented in [quarkus-qe/quarkus-openshift-test-suite](https://github.com/quarkus-qe/quarkus-openshift-test-suite)
  to verify functonality of Quarkus provided fabric8 client from within deployment in an OpenShift cluster (the operator 
  scenario)

### Impact on resources:
The new test implemented in Quarkus OpenShift test suite does not require infrastructural changes and its execution 
should be finishined within minutes. Test will be executed on all the tested OCP platforms.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the features

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature issue: [QUARKUS-713 Tech preview support for the OpenShift Client](https://issues.redhat.com/browse/QUARKUS-713).
* Upstream feature issue: [Implement OpenshiftClient on Kubernetes Client Extension #3200](https://github.com/quarkusio/quarkus/issues/3200)
* Upstream documentation: [Quarkus - Kubernetes Client # OpenShift client](https://quarkus.io/guides/kubernetes-client#openshift-client)
