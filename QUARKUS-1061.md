# QUARKUS-1061 - Tech Preview support for MongoDB

JIRA link: https://issues.redhat.com/browse/QUARKUS-1061

Initial analysis: https://issues.redhat.com/browse/QUARKUS-1061?focusedCommentId=18933696&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-18933696

https://quarkus.io/guides/mongodb

https://quarkus.io/guides/mongodb-panache

MongoDB datasource using `mongodb-client` and `mongodb-panache`.

## Scope of testing

### Existing tests
Quarkus integration tests:
- `mongodb-client`
    - `/vehicles` endpoint is not tested.
    - Coverage should be added back as it was present in older commits but removed in https://github.com/quarkusio/quarkus/pull/12902 (or at least we need to undestant why the test was removed, but not the classes relevant to the endpoint).
    - Native mode covered.
- `mongodb-panache`
    - Classic vs. reactive, transactions, various entities and repositories, native mode covered.
- `mongodb-rest-data-panache`
    - Various CRUD operations, json and hal+json output formats, native mode covered.

Quarkus Quickstarts:
- `mongodb-panache-quickstart` - covers related guide, nice starting point, includes tests for CRUD operations.
- `mongodb-quickstart` - covers related guide, reasonable starting point, not many changes in code, NO tests.

## Automated test development

- `mongodb-client` integration tests - reintroduce removed test coverage if possible.
- `mongodb-quickstart` - add tests.
- Update MongoDB instances used in integration tests and Quickstarts to use the newest versions of the database.
- New test in QE TS to try MongoDB scenario on OpenShift.
- Check why https://github.com/quarkusio/quarkus/blob/main/integration-tests/mongodb-rest-data-panache/src/test/java/io/quarkus/it/mongodb/rest/data/panache/MongoTestResource.java is used instead of https://github.com/quarkusio/quarkus/blob/main/test-framework/mongodb/src/main/java/io/quarkus/test/mongodb/MongoTestResource.java in mongodb-rest-data-panache.

## Impact on test suites and test environment
- One new scenario in QE TS, approximate time increase: 2 minutes JVM mode, 5 minutes native mode, 4 minutes OpenShift JVM, 8 minutes OpenShift native.
