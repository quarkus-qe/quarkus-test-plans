# QUARKUS-6335 IBM Maven Repo

JIRA: https://issues.redhat.com/browse/QUARKUS-6335

IBM Maven Repo


## Scope of testing
QUARKUS-6335 describes the setting of a new IBM maven repository for IBM Enterprise Build of Quarkus.
The testing of this new repository will be done only for first release of IBM Enterprise Build of Quarkus (3.27) to ensure the maven repository is working as expected.
Testing will be done in two phases.

First phase will test the maven repository before the general availability.
This maven repository will be only temporary due to the IBM release process.

Second phase will test the maven repository after the general availability to ensure that the maven repository works for customers.

Both phases will execute same tests which are described later in this TP.

In addition, the tests will be executed against Red Hat and maven central repository to check if the result are the same for IBM and Red Hat maven repository.
The execution against the maven central repository is to confirm that the setting of `setting.xml` behave as expected and the tests not work with the productized artifacts.
This will be done only as part of creating Jenkins jobs for the testing.


## Automated test development

There will be no permanent Jenkins jobs and test development, as the intended check is for the first release of the IBM Enterprise Build of Quarkus.

The temporary Jenkins jobs will be created.
These jobs will define the `setting.xml` in `~/.m2` directory.
The content of `setting.xml` will be set to use only the IBM maven repository.

All tests will be executed only in JVM mode as this smoke testing target functionality of maven repository instead of Quarkus.

### Selected test coverage

To test IBM maven repository the tests from [quarkus-test-suite](https://github.com/quarkus-qe/quarkus-test-suite) and [quarkus-startstop](https://github.com/quarkus-qe/quarkus-startstop) were selected.

The module selected from [quarkus-test-suite](https://github.com/quarkus-qe/quarkus-test-suite) is [super-size/many-extensions](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/super-size/many-extensions).

The tests selected from [quarkus-startstop](https://github.com/quarkus-qe/quarkus-startstop) are `generator` tests which use registry.
These tests will be checked for registry without set offering and with offering set to ibm.

All selected tests define a large number of extensions which should be sufficient for testing the IBM maven repository.


## References
- https://issues.redhat.com/browse/QUARKUS-6335

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>