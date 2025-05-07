# QUARKUS-5664 TLS - Enable Policy Configuration for Expired or Not Yet Valid Certificates

## Scope of testing
Verify, that the user can configure actions for expired or not-yet-valid certificates in TLS registry 

### Test cases
- verify, that all three possible values of `quarkus.tls.trust-store.certificate-expiration-policy` property ("ignore","warn" and "reject") are working as expected
- verify, that "warn" is a default one
- verify, that it works for both expired and not-yet-valid certificates
- verify, that it works in JVM and Native, create draft tests for Openshift as well (in case https://github.com/quarkus-qe/quarkus-test-framework/issues/1052 will be fixed).
- verify, that this behavior is consistent between certificate reloads
- verify, that there may be different TLS configs active in the same app

## Existing test coverage
No coverage

### Impact on test suite
New test should be added. I recommend to add it to either `security/https module`, or `http/rest-client-reactive`

## Impact on resources
Running time is expected to be between 1 (in JVM mode) to 6 (native mode) minutes. Additional RAM and CPU will not be required.

## References:
- jira issue: https://issues.redhat.com/browse/QUARKUS-5664
- the PR: https://github.com/quarkusio/quarkus/pull/45117
- The guide. There is no feature-specific guide yet, but I've asked Clement to add some notes about this feature https://quarkus.io/guides/tls-registry-reference 

## Contacts
* Tester: Fedor Dudinsky <fdudinsk@redhat.com>