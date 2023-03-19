# QUARKUS-3004: ARM support for JVM mode on OCP

Jira: https://issues.redhat.com/browse/QUARKUS-3004

The goal of this feature is to provide production support for Red Hat build of Quarkus running in JVM mode on OpenShift
on most commonly used server side 64-bit ARM architectures.

## Scope of the testing
To ensure same level of support for RHBQ 3 on OpenShift in JVM mode on ARM achitectures as on x86_64 architecture, 
Quarkus QE test suite OpenShift profile will have to pass with OpenJDK 17.

### Impact on test suites and testing automation
* OCP testings
  * OCP4 IPI install/destroy automation on AWS on ARM job will have to be implemented
  * A pipeline will have to be implemented in extended platforms that will setup OCP on ARM on AWS, run Quarkus QE 
    testsuite OpenShift profile on it and then clean it up
    * Cluster creation time: 20 minutes - 1 hour
    * Test suite OpenShift profile in JVM mode - ~1 hour
    * Cluster destruction time: ~10 minutes
  * Determine which of operators and testing services are supported on OCP on ARM
    * The plan is to implement ARM profile in Quarkus QE test suite which will contain containers for aarch64 providing
      the services we use in our test suite
    * In case we miss a service equivalent to what we have for x86_64, we will be in the same situation as for our 
      Windows support - this is a testing blocker, not a functional blocker
    * For missing operators, we will need to make a note about them in the documentation if they're not available by
      the time Red Hat build of Quarkus certifies.
  * Open questions
    * What is the supported source S2I container on aarch64
    * How do we run source S2I on OpenShift running out of Red Hat internal network with productized candidate artifacts 
    * What is the supported binary S2I container on aarch64
    * Red Hat OpenShift Serverless plans to support OCP on aarch64

### Impact on resources
* OCP testing
  * cluster - 6x xlarge Graviton machines in AWS
  * test suite - same machines in our Jenkins as for x86_64 OCP JVM testing (parallel pipeline, 8x large RHEL8 machines)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>
