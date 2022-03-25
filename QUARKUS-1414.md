# QUARKUS-1414 Service Binding improvements

Jira: https://issues.redhat.com/browse/QUARKUS-1414

The goal of this feature is to improve the provided technology preview support of OpenShift Service Binding in Quarkus
applications.

## Scope of the testing

### Impact on test suites and testing automation
* We need to ensure all the supported means of producing Quarkus deployments in OpenShift work with service binding
  * Source S2I
  * Docker build
  * Binary S2I
* Orchestration of OSD will have to be automated to ensure easy testing on OSD with RHOSAK service bindings. This, as of
  now, is done manually (see [QUARKUS-1027](https://issues.redhat.com/browse/QUARKUS-1027)).
* New tests have to be implemented in Quarkus QE test suite, with applications utilizing the following service bindings:
  * RHOSAK
  * Apicurio
  * MongoDB
  * MySQL
  * PostgreSQL

### Impact on resources
* A developer preview instance of RHOSAK will have used along with OpenShift Dedicated running in RHBQ QE AWS quota
  during the testing.
* Service binding operator will have to be installed in QE OpenShift clusters.
* Spin-up of the test applications (bindable services need to be instantiated) and test phase in test suite should take
  tens of minutes altogether.
* Implementation of test for RHBQ service binding with RHOSAK may take upwards of 5 working days
  * 2-3 days for orchestration and setup
  * 1 day for implementation of the orchestration in testing framework
  * 1 day for implementation of test in test suite

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature epic: [QUARKUS-1414 Service Binding improvements](https://issues.redhat.com/browse/QUARKUS-1414)
