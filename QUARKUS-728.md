# Quarkus-728 - Add support for OpenJDK 17

JIRA link: https://issues.redhat.com/browse/QUARKUS-728

The goal is to verify OpenJDK 17 integration

## Related test plans

 - [QUARKUS-1397](QUARKUS-1397)

## Scope of the testing
Test development will focus on  
 - New specifict scenarios will be created to cover the integration of new objects as “sealed” or “records” classes with several extensions as Hibernate or RESTEasy.
 - Current scenarios must work with Java 17
 - S2I (source to image) must works with Java 17
 - Non-S2I must work with Java 17
 - Code Quarkus could create an application pointing to Java 17
 - Quarkus-CLI and quarkus-maven-plugin must be able to create an application with Java 17
 - JDK17 brings a lot of interesting [JEPs](http://openjdk.java.net/jeps/1)  implemented since JDK 11, and these ones are the selected ones to be covered by this test plan:
    * [JEP 359, 384, 395: Records](https://openjdk.java.net/jeps/395)
    * [JEP 325, 354, 361: Switch Expressions](https://openjdk.java.net/jeps/361)
    * [JEP 353: Reimplement the Legacy Socket API](https://openjdk.java.net/jeps/353)
    * [JEP 355, 368, 378: Text Blocks](https://openjdk.java.net/jeps/378)
    * [JEP 305, 375, 394: Pattern Matching](https://openjdk.java.net/jeps/394)
    * [JEP 358: Helpful NullPointerExceptions](https://openjdk.java.net/jeps/358)
    * [JEP 360, 397, 409: Sealed Classes](https://openjdk.java.net/jeps/409)
    * [JEP 371: Hidden Classes](https://openjdk.java.net/jeps/371)

### Impact on testsuites and testing automation:
 - Ensure this coverage works in JVM and native mode
 - Ensure this coverage works in Openshift (S2I)
 - New test suite will be created in order to develop specifict Java 17 scenarios
 - Following RHBQ 2.x testing strategy Java 11 will be our primary platform that will support all the defined platforms, but Java 17 must also support a subsequent of these platforms:
   - RHEL 8 x86_64 with OpenJDK 17
   - Windows Server 2019 x86_64 with OpenJDK 17
   - OpenShift 3.11 with UBI8 OpenJDK 17
   - OpenShift 4.latest with UBI8 OpenJDK 17
   - OpenShift 4.LTS with UBI8 OpenJDK 17
   - OpenShift OSD with UBI8 OpenJDK 17
   - RHEL 9 x86_64 with OpenJDK 17
  
 That could be reflected in the following Jenkins jobs:

   - rhbq-2.2-extended-platform-testing-trigger
     - rhbq-2.2-baremetal-ts-jvm
     - rhbq-2.2-baremetal-ts-native
     - rhbq-2.2-rhel8-jdk17-openshift-ts-jvm-ocp-4-stable
     - rhbq-2.2-rhel8-openshift-ts-jvm-ocp
  
 - Lab capacity should be ideally increased by 50% in terms of CPU and RAM memory, if that doesn't happen the execution time of testing will be prolonged, almost doubled.
 - New Jenkins slaves that will run Java 17 pipelines will have OpenJDK 17 installed preferably an RHEL OpenJDK distribution.
 - Github actions should be configured in order to run tests over Java 11 and 17
    
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>