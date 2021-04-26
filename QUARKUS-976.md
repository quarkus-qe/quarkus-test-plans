# Quarkus-976 - Cover Hibernate ORM Rest Data with Panache extension
JIRA link: https://issues.redhat.com/browse/QUARKUS-976

Rest Data with Panache exist in flavors for HibernateORM and MongoDB. This test plan covers HibernateORM.

The goal is to verify proper functionality and enhance test coverage for hibernate-orm-rest-data-panache extension.

General overview of the extension functionality can be found at [this link](https://quarkus.io/guides/rest-data-panache#setting-up-rest-data-with-panache)


## Scope of the testing
Test development will focus on the supported features of the **hibernate-orm-rest-data-panache** extension.
For HibernateORM supported Panache Resources are:
- PanacheEntityResource
- PanacheRepositoryResource

These resources should be extended and based on interfaces.

Resource customization feature is provided through setting up parameters of the following annotations:
- @ResourceProperties
- @MethodProperties

The test will verify correct handling of CRUD operations that are customized by resource and method properties.

### Impact on testsuites and testing automation:
 - Enhance existing beefy-scenarios test case
 - Ensure this coverage works both in JVM and NATIVE mode 

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use

