# QUARKUS-821 Early checks of code.quarkus as part of Acceptance pipeline

JIRA link: https://issues.redhat.com/browse/QUARKUS-821

The goal is to ensure pre-stage version of product code.quarkus site is using latest productized bits and is delivered as part of the product build handoff.

## Scope of the testing

Tests for Acceptance pipeline will cover certain areas of the pre-stage version of product code.quarkus site to ensure Red Hat branding is applied and latest product bits are used.

### Impact on test-suites and testing automation:
 - Start-Stop test-suite will be enhanced to cover new scenarios
 - New job will be created and included into Acceptance pipeline
 - Impact on lab capacity is one medium size executor and up to 20 minutes of execution time

### Details of the testing

**CodeQuarkusSiteTest**

 - add tests for:
   - code.quarkus pre-stage site is available
   - code.quarkus is Red Hat branded (e.g. logo in top left corner)
   - code.quarkus has product flags (e.g supported keyword search or class value search)
   - expected BOM version is shown in top right corner (e.g. 1.11.6.Final-redhat-00001)
 - execute available tests including checks for
   - favicon.ico presence
   - page title refers to code.quarkus.redhat.com

**CodeQuarkusTest**

 - identify subset of supported extensions for acceptance tests
 - compile the application with selected subset of supported extensions
 - run the application in dev mode to ensure proper functionality
 - preferably `-Dtest=CodeQuarkusTest#supportedExtensionsSubsetA`


## Automated test development
All the tests will be automated, included into https://github.com/quarkus-qe/quarkus-startstop repository.
