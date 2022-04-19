# QUARKUS-1251 Test RHBQ on OpenShift Dedicated

## Scope of the testing

### Impact on test suites and testing automation
* A job preparing an OSD cluster with configuration required for Quarkus QE test suite to be ran should be implemented.
* A job should be implemented that runs the Quarkus QE test suite on the installed OSD cluster in JVM and native mode as
  a part of the extended platform pipeline.
* OSD as a platform should be added as OpenShift version to Polarion, and the jobs should be included in the existed
  Polarion JVM and native test suite work items.

### Impact on resources
* A medium executor for JVM mode test suite run for up to 4 hours
* A large executor for native mode test suite run for up to 8 hours
* A standard OSD cluster in AWS for a day per a promoted release

## Future considerations
* Possibility of non-interactive installation of OSD should be investigated
  * Tools coming to mind are `ocm` CLI or Flexy

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature request: [QUARKUS-1251 Test RHBQ on OpenShift Dedicated](https://issues.redhat.com/browse/QUARKUS-1251)