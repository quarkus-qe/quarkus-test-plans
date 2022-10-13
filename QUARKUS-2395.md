# QUARKUS-2395 - Drop TP from Security JPA extension

JIRA: https://issues.redhat.com/browse/QUARKUS-2395

The extension has tech-preview support in RHBQ 2.7. Starting with RHBQ 2.13 the tech-preview support is dropped.
The extension stays available as the community extension with community-level support.

## Scope of the testing

Metadata related to the extension need to be dropped and not visible on https://code.quarkus.redhat.com/ pre-stage instance.

References to the extension should be replaced in relevant modules of Quarkus QE test suite.

Marete and StartStopTS need to be checked and adjusted to reflect the drop of the TP level.

### Impact on resources
- No change in needed resources for testing
