# QUARKUS-1413 - Service Discovery with Stork

JIRA link: https://issues.redhat.com/browse/QUARKUS-1413

This test plan cover SmallRye Stork Quarkus integration (Technical Preview).

## External resources

 - [Using Stork with Quarkus](http://smallrye.io/smallrye-stork/1.0.0.Beta1/quarkus/)
 - [Kubernetes Service Discovery and Selection with Stork](https://quarkus.io/blog/stork-kubernetes-discovery/)
 - [Quarkus Insights #70: Introducing Stork](https://www.youtube.com/watch?v=l3mLKU3wR2A)
 - [stork-quickstart](https://github.com/quarkusio/quarkus-quickstarts/tree/development/stork-quickstart)
 - [Stork Getting-started guide](https://github.com/quarkusio/quarkus/pull/22592)
  
## Scope of the testing

SmallRye Stork provides service discovery and client-side load balancing features. 

Test development will focus on
- Service Discovery
  - Service Discovery in OCP/k8s
  - Custom service discovery mechanism
- Load-Balancing
  - Round-Robin Load Balancing in OCP/k8s
  - Custom load balancer mechanism

### Impact on testsuites and testing automation:

   New Stork modules need to be created in Quarkus test suite in order to cover these scenarios. Most of the tests are going to be OpenShift scenarios that take more time to be executed than a baremetal test. The main impact is related to increase test suite total amount of time execution. 
   
   We are going to combine service discovery with load balancing in one scenario, so two scenarios will be required. We estimate that each scenario will take around 3 min in JVM mode, and 8 min in native mode, so overall the test suite will be increased to 6 min for JVM mode and 16 min for native mode. 
  
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Exploratory testing following getting started guides 
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>