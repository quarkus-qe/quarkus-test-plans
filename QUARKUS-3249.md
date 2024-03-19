# QUARKUS-3249 Configurable init container

This document is a high level test plan for options to configure init containers in Quarkus 

JIRA link: https://issues.redhat.com/browse/QUARKUS-3249

## Scope of the testing
* Check, that flyway task can be disabled
* Check, that we can use custom image for the run and change its pull strategy
  * I suppose, we should build the image ourselves, so a Docker file should be provided 

### Current test coverage
* Upstream coverage for Kubernetes, but without runs on the cluster

### Impact on test suites and testing automation
* Several new tests (probably, one or two new test classes) in `sql-db/panache-flyway` module

### Impact on resources
* No impact on maximum consumed cluster resources
* Openshift JVM run will take five or ten more minutes for databases scenario, Openshift Native run will take twice as much
  * Databases scenario is still shorter, than http-scenario, so no effect on total run time
  
## Future considerations
* Check, that Quarkus uses better image (no image agreed in upstream yet) 

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@redhat.com>

## References
* Feature JIRA: [QUARKUS-3249 Make the init container image configurable and find a A+ image](https://issues.redhat.com/browse/QUARKUS-3249)
* Upstream docs: https://quarkus.io/guides/init-tasks
* Upstream issue: https://github.com/quarkusio/quarkus/issues/35455
