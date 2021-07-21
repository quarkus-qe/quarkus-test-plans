# Quarkus-728 - Add support for OpenJDK 17

JIRA link: https://issues.redhat.com/browse/QUARKUS-728

The goal is to verify OpenJDK 17 integration

## Scope of the testing
Test development will focus on  
 - New specifict scenarios will be created to cover the integration of new objects as “sealed” or “records” classes with several extensions as Hibernate or RESTEasy.
 - Current scenarios must work with Java 17
 - S2I (source to image) must works with Java 17  

### Impact on testsuites and testing automation:
 - Ensure this coverage works in JVM mode
 - NATIVE mode is not required in Java 17 
 - Ensure this coverage works in Openshift (S2I)
 - New test suite will be created in order to develop specifict Java 17 scenarios
 - New test suite will use Gradle as a build tool
 - Following RHBQ 2.x testing strategy Java 11 will be our primary platform that will support all the defined platforms, but Java 17 must also support a subsequent of these platforms:
   - RHEL 8 x86_64 with OpenJDK 17
   - Windows Server 2019 x86_64 with OpenJDK 17
   - OpenShift 3.11 with UBI8 OpenJDK 17
   - OpenShift 4.latest with UBI8 OpenJDK 17
   - OpenShift 4.LTS with UBI8 OpenJDK 17
   - OpenShift OSD with UBI8 OpenJDK 17
   - RHEL 9 x86_64 with OpenJDK 17
  
 That could be reflected in the following Jenkins jobs:

   - RHBQ
     - rhbq-1.11-rhel8-jdk11-beefy-ts-jvm
     - rhbq-1.11-rhel8-jdk11-openshift-ts-jvm-ocp-4
     - rhbq-1.11-rhel8-jdk11-quickstarts-ts-jvm-quarkus-bom
     - rhbq-1.11-rhel8-jdk11-quickstarts-ts-jvm-quarkus-universe-bom
     - rhbq-1.11-rhel8-jdk11-startstop-ts
     - rhbq-1.11-rhel8-jdk11-startstop-ts-acceptance-code-quarkus
     - rhbq-1.11-rhel8-jdk11-startstop-ts-boms
     - rhbq-1.11-rhel8-jdk11-startstop-ts-code-quarkus
     - rhbq-1.11-rhel8-jdk11-startstop-ts-generator
     - rhbq-1.11-rhel8-jdk11-startstop-ts-special-chars
     - rhbq-1.11-rhel9-jdk11-startstop-ts
     - rhbq-1.11-rhel9-jdk11-startstop-ts-acceptance-code-quarkus
     - rhbq-1.11-rhel9-jdk11-startstop-ts-boms
     - rhbq-1.11-rhel9-jdk11-startstop-ts-code-quarkus
     - rhbq-1.11-rhel9-jdk11-startstop-ts-generator
     - rhbq-1.11-rhel9-jdk11-startstop-ts-special-chars
     - rhbq-1.11-rhel9-jdk11-beefy-ts-jvm
     - rhbq-1.11-rhel9-jdk11-openshift-ts-jvm-ocp-4
     - rhbq-1.11-rhel9-jdk11-quickstarts-ts-jvm-quarkus-bom
     - rhbq-1.11-rhel9-jdk11-quickstarts-ts-jvm-quarkus-universe-bom
     - rhbq-1.11-win19-jdk11-quickstarts-ts-jvm-quarkus-bom 
     - rhbq-1.11-win19-jdk11-quickstarts-ts-jvm-quarkus-universe-bom 
     - rhbq-1.11-win19-jdk11-beefy-ts-jvm 
     - rhbq-1.11-win19-jdk11-openshift-ts-jvm-ocp-4-stable-deployment-strategy 
  
  **Note:** The final list of Jenkins jobs that are going to be supported by Java 17 need to be discussed by QE team. 
  
 - Lab capacity should be ideally increased by 50% in terms of CPU and RAM memory, if that doesn't happen the execution time of testing will be prolonged, almost doubled.
 - New Jenkins slaves that will run Java 17 pipelines will have OpenJDK 17 installed preferably an RHEL OpenJDK distribution but if doesn't exist because is an early stage version then we will use an SDKman distribution
 - Github actions should be configured in order to run tests over Java 11 and 17
    
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>