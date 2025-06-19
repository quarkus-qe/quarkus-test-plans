# QUARKUS-5661 Bump kubernetes-client-bom from 6.13.4 to 7.0.1

JIRA: https://issues.redhat.com/browse/QUARKUS-5661

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/45259

## Scope of the testing
- Verify, that there is no functional regressions after the update

## Existing test coverage
* Kubernetes-client is used (as a transitive dependency) in all Quarkus QE TS and FW OpenShift tests

### Impact on test suites and testing automation
None. Current coverage should be enough.
 
### Impact on resources
None

## Contacts
* Tester: Fedor Dudinskii <fdudinsk@redhat.com>
