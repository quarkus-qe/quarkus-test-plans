# QUARKUS-1472 Java version support in tooling

JIRA link: https://issues.redhat.com/browse/QUARKUS-1472

Test plan covers `auto-detect` java version support tooling.

## Related test plans

- [Quarkus-728](Quarkus-728.md)

## Scope of the testing
Verify that a generated project detect host java version, and these version is used into the selected build tool and docker.

## Impact on test suites and testing automation

New scenarios are going to be developed into Quarkus test suite / quarkus-cli module. The impact should be low, because the verification is focus in
check that Maven/Docker is pointing to the right Java version, so should have a minimal impact. 

## Automated test development
- Update existing `QuarkusCli create app` scenarios in order to check the Java version

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>
