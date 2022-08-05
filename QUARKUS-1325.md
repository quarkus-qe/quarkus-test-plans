# QUARKUS-1325 - Helm chart support for Quarkus

JIRA link: https://issues.redhat.com/browse/QUARKUS-1325

Quarkus Guide: https://quarkiverse.github.io/quarkiverse-docs/quarkus-helm/dev/index.html

This test plan cover Quarkus - Helm integration

## Scope of the testing

Verify a repeatable/versioned way to deploy Quarkus application into K8s/ocp through helm. We are assuming that the user is going to follow the Helm standard release flow, which means that the user will push his Quarkus app to his configured Docker registry and then this docker image is going to be used by the generated `chart.yaml`. This `build app -> push app to docker registry -> Helm chart generation` is done through `quarkus-helm` + `quarkus-container-image-docker` as is explained in Quarkus Helm extension [documentation](https://quarkiverse.github.io/quarkiverse-docs/quarkus-helm/dev/index.html). 

The deployment to K8s/OCP is done as a regular Helm chart. Ex `helm -f app-chart.yaml sync`

Test development will focus on
- JVM mode. We have internal conversations about the convenience of verifying JVM and native executions and we've concluded that Helm is concerned just about image name and doesn't care if the app is JVM or Native based. For this reason and in order to save resources we are only going to cover JVM mode.
- Generate and deploy a Quarkus Helm Chart over OpenShift default repository
- Deploy app container image into quay.io
- Verify some mapping between Quarkus configuration and generated Helm chart:
  - Quarkus SmallRye Health 

### Impact on testsuites and testing automation:

  - A new module will be created on Quarkus test-suite in order to cover this scenarios
  - Jenkins slaves instances will have Helm / helmfile installed and will deploy a Quarkus application through Helm into an OpenShift cluster instance. This is a new way to deploy into K8s/OpenShift that will increase slightly the total execution of time of Quarkus test-suite. Native execution should be avoided, because we don't see any additional value for these scenarios.  
  - Charts should be removed from OpenShift default repository after test execution 
  
## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Exploratory testing with current Quarkiverse extension
 - Ensure upstream CI coverage
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>