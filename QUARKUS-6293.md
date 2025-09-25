# QUARKUS-6293 - Show support status for the chosen product in code.quarkus

JIRA: https://issues.redhat.com/browse/QUARKUS-6293

Show support status for the chosen product in code.quarkus

Upstream PRs:
- https://github.com/redhat-developer/code.quarkus.redhat.com/pull/190
- https://github.com/redhat-developer/code.quarkus.redhat.com/issues/183
- https://github.com/quarkusio/code.quarkus.io/pull/1656
- https://github.com/quarkusio/code.quarkus.io/pull/1652

## Scope of testing

This feature is part of [QUARKUS-6292 (Extend the platform with offering)](https://issues.redhat.com/browse/QUARKUS-6292) feature.
You can find the test plan at https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6292.md.

Update of Code Quarkus is defined in https://issues.redhat.com/browse/QUARKUS-6293.
There will be separate Code Quarkus for each offering (e.g. each offering will have own URL).
The Code Quarkus will show the support level only for the self offering, other extensions will be marked as community.
Newly it is possible to filter by support levels, which are defined for each offering (the support level for each offering can be different)

In addition, the Code Quarkus add possibility to search for extension with some scope of support or search only for the extension without any support.


## Automated test development

The start-stop testsuite already contains the tests for Code Quarkus.
These tests are cover only RedHat Code Quarkus, which will be used to test `redhat` offering.
The tests for IBM Code Quarkus needs to be added.
The IBM tests will be modification of the `CodeQuarkusSiteTest`.
Also, the generic tests for Code Quarkus in `CodeQuarkusTest` will be executed for both RedHat and IBM Code Quarkus.
The `CodeQuarkusTest` testing downloading the zip and executing the downloaded project.
The url to download zip can be set using `code.quarkus.url`

In addition, the tests to show only supported extension and show only unsupported extension will be implemented.

The jenkins jobs will be updated to `matrixJob` to run both RedHat and IBM tests simultaneously.
The start-stop profiles will run by default all tests except IBM tests.
To execute IBM tests, the profile `ibm` will be added to start-stop testsuite and will exclude the RedHat specific tests.

The daily tests runs only against code.quarkus.redhat.com.
After the code.quarkus for IBM is available for public use, the daily tests will be expanded to run against the IBM code.quarkus.


### Impact on TS and resources
The test will be run only on baremetal in JVM mode.
Total execution time increase should be in a few minutes maximum.


## References
- https://issues.redhat.com/browse/QUARKUS-6292
- https://issues.redhat.com/browse/QUARKUS-6293

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
