# QUARKUS-3290 - Generate source zip which contains the source code of the built artifacts

JIRA: https://issues.redhat.com/browse/QUARKUS-3290

As part of discussions abound QUARKUS-2701, it was decided to generate source zip which contains the source code of the built Maven artifacts, sources will be pulled from the internal Gerrit repository for upcoming 3.x releases.

One of the reasons is to fully comply with Complete Corresponding Source Code recommendation from Red Hat compliance training [1].

Another big reason is to demonstrate the amount of work that is done during the productization effort - should help PM in his discussions and with the secure build chain efforts.

## Scope of the testing
 * Exploratory testing
 * Evaluation of existing test scenarios
 * New test development

Topics for exploratory testing:
 * SRC ZIP naming
 * Root directories naming
 * Sources for platform participants
 * Review of included sources for transitive dependencies
 * Files count, summary per file extension
 * Large files check
 * Empty files and directories check

### Impact on test suites and testing automation
 * Marete checks for source zip need to be reworked
   * Adjustments of the tests based on the experience from the exploratory testing and feedback in filed JIRAs
     * These changes need to be communicated with other Marete users
   * Adjustments of the configuration based on the experience from the exploratory testing and feedback in filed JIRAs

### Impact on resources
No need for additional resources is anticipated at this moment. There is a possibility of increased memory needs as the SRC ZIP size is several hundreds of megabytes, depending on the amount of data kept in memory during the checks.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>

## Resources
[1] https://issues.redhat.com/secure/attachment/12796750/screenshot-1.png
