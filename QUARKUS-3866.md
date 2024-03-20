# QUARKUS-3866 Dev support and productization for quarkus-security-jpa
- JIRA link: https://issues.redhat.com/browse/QUARKUS-3866
- Quarkus Guides: 
  - https://quarkus.io/guides/security-getting-started-tutorial
  - https://quarkus.io/guides/security-jpa

## Scope of the testing
- Exploratory testing of the feature
- Verify dev mode support for the extension
- Test coverage in QQE TS not only using H2 and PostgreSQL databases for the identity store
- Test coverage in QQE TS using custom password providers with different hashing algorithms

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Existing test coverage
- [Integration tests in Quarkus upstream](https://github.com/quarkusio/quarkus/tree/main/extensions/security-jpa/deployment/src/test/java/io/quarkus/security/jpa)
  
  Basic authentication mechanism and H2 database. Tests access resource with/no authentication and authorization, verify error codes.
  - Bcrypt password mapper
  - Custom password mapper
  - Multiple entities
  - Multiple roles
  - Multiple roles in collection
  - Multitenant persistence
  - Named persistence 

- [Integration tests in Quarkus Quickstarts](https://github.com/quarkusio/quarkus-quickstarts/tree/main/security-jpa-quickstart/src/test/java/org/acme/elytron/security/jpa)

  Basic RBAC tests with PostgreSQL database described in [Quarkus guide](https://quarkus.io/guides/security-getting-started-tutorial#test-your-application-by-using-dev-services-for-postgresql)
- In Quarkus QE projects we have no tests for this extension 

### Impact on test suites and testing automation
- New tests will be added inside [security](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/security) directory in QQE TS.

### Impact on resources
- There will be longer tests execution for JVM and native runs of QE TS.

## Contacts
- Tester: Georgii Troitskii <gtroitsk@redhat.com>