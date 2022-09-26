# Quakrus-714 - Tech preview of compatibility layer for Spring Data REST
Test plan covers the integration of the Spring Data REST extension with Quarkus, which is used to expose hypermedia-driven REST web services.  
[This](https://www.baeldung.com/spring-data-rest-intro) is a general overview of the Spring Data REST

JIRA link: https://issues.redhat.com/browse/QUARKUS-714


## Scope of the testing
Test development will focus on the supported features of the **quarkus-spring-data-rest** extension.  
Currently supported Spring Data repositories:
 - CrudRepository
 - PagingAndSortingRepository
 - JpaRepository

 Currently supported annotation properties:
  - exported, path, collectionResourceRel

Details about different aspects regarding Spring Data REST usage can be found in https://www.baeldung.com/spring-data-rest-intro and https://github.com/eugenp/tutorials/tree/master/spring-data-rest  
These resources will be processed and further interpreted for Quarkus based usage where possible. Switch to Quarkus is expected to be smooth.


### Impact on testsuites and testing automation:
 - New test case for beefy-scenarios with Spring experience
 - Ensure this coverage works both in JVM and NATIVE mode 


## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use
 
## Automated test development
This step should ensure:
 - Focus on high level scenarios with multiple components and features involved
 - Ensure platform specific features or components have appropriate test coverage

