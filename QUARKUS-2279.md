# QUARKUS-2279 - Productization of JOSDK

JIRA: https://issues.redhat.com/browse/QUARKUS-2279

Triggered by the need of Keycloak team as they are planning on releasing product version based on Quarkus including an operator using JOSDK. 

JOSDK bits will be delivered as part of the Platform, marked as Developer Preview (https://access.redhat.com/support/offerings/devpreview) so the bits can be productized.

## Scope of the testing
 * RHBQ QE team will ensure availability of the JOSDK artifacts and platform BOM
 * Functional testing of the use-cases is on Keycloak side

### Impact on test suites and testing automation
 * Marete checks for maven repo zip will be extended to ensure availability of the JOSDK artifacts and platform BOM

### Impact on resources
No need for additional resources.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>
