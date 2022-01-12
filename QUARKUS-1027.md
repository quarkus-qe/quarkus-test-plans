# QUARKUS-1027 OpenShift Service Binding - Tech preview

Jira: https://issues.redhat.com/browse/QUARKUS-1027

The goal of this feature is to provide technology preview support of OpenShift Service Binding for RHOSAK in Quarkus 
applications.

## Scope of the testing

### Impact on test suites and testing automation
* None. The testing is executed manually for the RHBQ 2.2 releases by deploying Quarkus Kafka Quickstart in OpenShift 
  Dedicated and binding it to a running RHOSAK instance.
* We need to ensure all the supported means of producing Quarkus deployments in OpenShift work with service binding
  * Source S2I
  * Docker build
  * Binary S2I

### Impact on resources
* Currently, a developer preview instance of RHOSAK is used along with OpenShift Dedicated running in RHBQ QE AWS quota
  during the manual testing.

## Future considerations
There is a plan to improve Quarkus support of OpenShift service bindings in future releases (see [QUARKUS-1414](https://issues.redhat.com/browse/QUARKUS-1414)).
From the QE point of view, the future tasks on the way to full support must include
* Automation of orchestration of OSD and RHOSAK and implement a test in Quarkus test suite deploying a demo application
  (Quarkus Kafka Quickstart) and binding it to RHOSAK.
* Binding to other services.

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature epic: [QUARKUS-1027 OpenShift Service Binding - Tech preview](https://issues.redhat.com/browse/QUARKUS-1027)
