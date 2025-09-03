# QUARKUS-5881 - Datasource is too slow to recover when a DB failover occurs

JIRA: https://issues.redhat.com/browse/QUARKUS-5881

Upstream issue: https://github.com/quarkusio/quarkus/issues/47091

Upstream PR: https://github.com/quarkusio/quarkus/pull/47233

Quarkus 3.22 added an option to configure a timeout for the query executed to validate a connection.
The timeout allows for quicker eviction of unresponsive connections.
When the timeout is not configured, the default socket timeout is used.
Previously, validation queries could block access to the connection pool when terminated connections were encountered.

## Scope of the testing

Verify that the timeout for the validation query is applied when the database is unreachable.
We will test MariaDB, MySQL, SQL Server, Oracle, and PostgreSQL.

## Existing test coverage

Upstream test coverage uses H2 database and only checks that the `SET QUERY_TIMEOUT ?` command is set.
It does not test that the timeout is actually enforced.

### Impact on test suites and testing automation

The `io.quarkus.ts.transactions.TransactionCommons` test class parent in the `sql-db/narayana-transactions` module will be enhanced to stop the database and test the validation query timeout.

### Impact on resources

Roughly 30 seconds for each database and supported configuration is required:

* 5 databases * JVM mode = 2.5 minutes
* 5 databases * native mode = 2.5 minutes
* 5 databases * OpenShift and JVM mode = 2.5 minutes
* 5 databases * OpenShift nd native mode = 2.5 minutes

That is 10-minute increase in the test execution time.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [JBJCA-1402: Failure to set query timeout for validation may result in long blocks of the pool](https://issues.redhat.com/browse/JBEAP-18712)
