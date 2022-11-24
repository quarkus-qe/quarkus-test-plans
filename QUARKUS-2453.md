# QUARKUS-2453 - Deprecate quarkus-reactive-routes extension

JIRA: https://issues.redhat.com/browse/QUARKUS-2453

Deprecate `quarkus-reactive-routes` extension starting with RHBQ 2.13, for RHBQ 2.7 the extension is on supported level.

## Scope of the testing

Ensure `deprecated` redhat-support label is rendered for quarkus-reactive-routes extension on https://code.quarkus.redhat.com/ pre-stage instance.

Marete and StartStopTS need to be checked and adjusted to reflect the drop of the Supported level to the Deprecated one.
Main Quarkus QE TS won't be adjusted, test coverage needs to stay in place till the support is dropped for good.

### Impact on resources
- No impact on resources for TS
