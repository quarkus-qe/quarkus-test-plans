# QUARKUS-2332 - Drop TP support for the Reactive DB2 client

JIRA: https://issues.redhat.com/browse/QUARKUS-2332

The extension has tech-preview support in RHBQ 2.7. Starting with RHBQ 2.13 the tech-preview support is dropped.
The extension stays available as the community extension with community-level support.

## Scope of the testing

Metadata related to the extension need to be dropped and not visible on https://code.quarkus.redhat.com/ pre-stage instance.

References to the extension should be replaced in relevant modules of Quarkus QE test suite.

Marete and StartStopTS need to be checked and adjusted to reflect the drop of the TP level.
Main Quarkus QE TS will be adjusted to no longer cover the extension.

### Impact on resources
- Quarkus QE TS execution time should be reduced by 4-5 minutes on JVM mode, 8-10 minutes on Native mode
