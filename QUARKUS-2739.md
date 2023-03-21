# QUARKUS-2739 - Add support for JDBCStore transaction logs

JIRA: https://issues.redhat.com/browse/QUARKUS-2739

Add support for `JDBC journal transaction logs`.

## Scope of the testing

A database transaction log should record all transactions and the database modifications made by each transaction. 
If there is any fatal system failure, this log should be enough to recover your database back to a consistent state.

Ensure that the Narayana extension deals well with JDBC datasource transaction log and that all the pending transactions (due to a crash
on the Quarkus app) are completed when the application recovers. 

Tests should verify:
- All supported datasources
    - Postgres
    - MariaDB
    - Mysql 
    - Mssql
    - Oracle
- JVM and Native mode
- Schema creation control would need to be configurable; log/generate the SQL scripts related to the missing required tables
- OpenShift integration (Operator is not planned for the initial release)
- Verify the documentation related to "crash recovery procedure"

### Existing tests

On Quarkus test suite there is already a Narayana module that will be reused in order to verify `JDBCStore transaction logs`.

### Impact on resources

Required additional time for the test execution should be around 10 minute for native platform and 5 min for JVM. In case the test runs over OpenShift, another 2 min needs to be added in order to complete the source to image deployment. Native additional time is due to the native compilation, that is going to be done in a completly new scenario decouple of the current scenarios. It is important to have a new scenario because the test is going to monitor the JBOSSTSTXTABLE table where all the journal entries are stored.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use
- Get familiar with JDBC store test plan and test coverage for EAP product
- Search JIRA for JDBC store issues tracked for EAP product to learn from previous findings

## Contacts

* Tester: Pablo Gonzalez <pagonzal@redhat.com>