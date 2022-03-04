# QUARKUS-1164 - Tech Preview Support for Oracle JDBC Extension

JIRA link: https://issues.redhat.com/browse/QUARKUS-1164

Initial analysis: https://issues.redhat.com/browse/QUARKUS-1164?focusedCommentId=18935716&page=com.atlassian.jira.plugin.system.issuetabpanels:comment-tabpanel#comment-18935716

https://quarkus.io/guides/datasource

Oracle datasource using `jdbc-oracle` extension.

## Scope of testing
- Connect to a database and persist data using Hibernate ORM with Panache.

### Existing tests
- `jpa-oracle` module in Quarkus upstream: https://github.com/quarkusio/quarkus/tree/main/integration-tests/jpa-oracle
  - CRUD operations.
  - Invocation of a DB procedure.

## Automated test development
- Add Oracle scenario into existing module `sql-db/sql-app` in https://github.com/quarkus-qe/quarkus-test-suite/tree/main/sql-db/sql-app.
- Add a quickstart - submit an issue to create a dedicated quickstart module.
- Documentation at https://quarkus.io/guides/datasource
  - https://quarkus.io/guides/datasource#jdbc-url - the section is missing a description of the Oracle JDBC URL schema and a link to Oracle documentation.
  - Submit an issue to amend the document.

## Impact on test suites and test environment
- Both upstream integration tests and planned Oracle scenario in `sql-db/sql-app` use OracleDB 18 express edition (gvenzl/oracle-xe:18.4.0-slim). This container takes several minutes to boot and is 2.5 GB at download time. Affected test suites:
  - 1 new scenario in QE TS
  - 1 new quickstart
