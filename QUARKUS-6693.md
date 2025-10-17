# QUARKUS-6693: Drop support of quarkus-derby

Jira: https://issues.redhat.com/browse/QUARKUS-6693

This story is about dropping deprecated extension `quarkus-jdbc-derby`.

## Scope of the testing

### Upstream testing
* This only affects Red Hat build of Quarkus

### Impact on test suites and testing automation
* The extension will be removed from list of expected extensions with support 
  label in Marete.
* We do not have any tests using `quarkus-jdbc-derby` otherwise.

### Impact on resources
* No impact, no new tests added.

## References
* Feature story: [QUARKUS-6693: Drop support of quarkus-derby](https://issues.redhat.com/browse/QUARKUS-6693)

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>