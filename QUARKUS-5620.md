# QUARKUS-5620 Add new AuthorizationPolicy annotation to bind named HttpSecurityPolicy to a Jakarta REST endpoints
## Links to the resources

JIRA: https://issues.redhat.com/browse/QUARKUS-5620
PR: https://github.com/quarkusio/quarkus/pull/42749
Issue: https://github.com/quarkusio/quarkus/issues/42710
Docs: https://quarkus.io/guides/security-authorize-web-endpoints-reference#custom-http-security-policy

## Scope of the testing
- Check that we can use the custom policy.
- Check that if the policy can not be found (eg due to typo), Quarkus fails with clear error
- Check, what will happen if there are two policies: one applied via annotation and another via properties


## Automated test development
New test methods should be added into the existing tests, probably into BasicSecurityIT in the basic/security module.
This will guarantee coverage in all four modes and minimally affect the running time and required resources.
 
# Feature status:
Affected extensions (REST and RestEasy) are fully supported.


## Contacts
* Tester: Fedor Dudinskii <fdudinsk@redhat.com>
