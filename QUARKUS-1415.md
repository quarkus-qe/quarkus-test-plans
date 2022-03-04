# QUARKUS-1415 Reactive REST Client improvements

JIRA link: https://issues.redhat.com/browse/QUARKUS-1415

Test plan covers Reactive REST Client functionality provided by `quarkus-rest-client-reactive` extension.

Documentation:
https://quarkus.io/version/main/guides/resteasy-reactive
https://quarkus.io/version/main/guides/rest-client-reactive

## Related test plans
- [QUARKUS-1075](QUARKUS-1075.md)

## Scope of the testing
- Upload and download of large (more than 2Gb) files, see https://github.com/quarkusio/quarkus/pull/22229 for details
- Support for proxy authentication
- New configuration schema introduced in https://github.com/quarkusio/quarkus/pull/17220
- Recursive sub-resources (see https://github.com/quarkusio/quarkus/pull/22417)
- Support for ParamConverter (related issue: https://github.com/quarkusio/quarkus/issues/23486)
- Manual check of Dev UI 

## Impact on test suites and testing automation:
- Four new scenarios in QE TS
- Upload and download of large (more than 2GB) files will add several minutes to the execution time
- Additional impact for Native mode is expected to be around 4 minutes per new scenario
- Overall time execution increase is ~20 minutes

## Automated test development
- Identify existing coverage in Quarkus integration tests.
- Update existing RESTEasy Reactive scenarios

## Advanced topics for test development
- Automated tests for Dev UI
