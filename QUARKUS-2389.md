# QUARKUS-2389 - Promote OpenID Connect Client Filter Reactive from TP to full support

Evaluate the extension for promotion to full support. Propose needed enhancements and report found bugs. 

## Scope of the testing
 - Review of test coverage in Quarkus integration tests
 - Review Quarkus Quickstarts which are using the extension
 - Review upstream guides for the extension
 - Exploratory testing of the extension
 - Migrate application using non-reactive filter to reactive one
   - Identify the usability issues and migration obstacles 
 - Implement test coverage for the extension in QE test suite

### Impact on test suites and testing automation
 - New module in https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security
 - Focus on parity between non-reactive and reactive variants
 - No impact on test automation is expected

### Current test coverage
#### Quarkus repo
```bash
grep "@Test" extensions/oidc-client-reactive-filter -R | wc -l
      0

grep "@Test" integration-tests/oidc-client-reactive -R | wc -l
       3
```
#### Quarkus QE test-suite repo
One reproducer for issue fixed in RHBQ .z stream
```bash
grep "@Test" security/keycloak-oidc-client-reactive -R | wc -l
      1
```

## Automated test development
 - New module in https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security
 - GitHub Issues or Pull Requests in Quarkus main repo for test coverage enhancements
 - Optionally GitHub Issues or Pull Requests in Quarkus Quickstarts repo for test coverage enhancements

## Links
* https://issues.redhat.com/browse/QUARKUS-2389
* https://quarkus.io/guides/security-openid-connect-client
* https://quarkus.io/guides/security-openid-connect-client-reference#oidc-client-reactive-filter
