# QUARKUS-6243 - Dedicated Dev UI interface to execute HQL (Hibernate ORM) queries

JIRA: https://issues.redhat.com/browse/QUARKUS-6243

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/46728
- https://github.com/quarkusio/quarkus/pull/49165 

PR #46728 introduces a new HQL Console in the Quarkus Dev UI for Hibernate ORM.
It allows developers to select Hibernate persistence units and their entities, displaying existing data and executing custom HQL queries.

## Getting familiar with the feature
- Understand how the HQL Console integrates in Dev UI using video preview from PR #46728: 
  - Query execution 
  - Display of results
  - Display of error messages
- Review upstream test coverage provided as part of #46728

## Scope of the testing
Coverage will target at least two different databases.

Scenarios to be included:
- Valid query execution
  - Insert test entities and run a valid HQL query
  - Assert JSON response contains expected rows

- Invalid query
  - Submit an invalid HQL query and assert returned error message

- Nested/complex values
  - Some entities have associations or collections (e.g. `Order` entity with associated `Address` and `OrderItem`)
  - Verify results are serialized as structured JSON (nested objects/arrays) without serialization errors

- Pagination
  - Insert more rows than a single page can hold
  - Verify that multiple pages of results can be retrieved correctly

### Impact on test suites and testing automation
The `sql-db/hibernate` module in the TS will be extended with new tests covering the above scenarios.

### Impact on resources
The expected execution time will increase by several minutes in JVM mode. Other scenarios (e.g. Native, OpenShift) are not relevant in this case.

## Contacts
* Tester: Georgii Troitskii <gtroitsk@ibm.com>

## References
- https://docs.jboss.org/hibernate/orm/7.1/userguide/html_single/Hibernate_User_Guide.html#query-language
