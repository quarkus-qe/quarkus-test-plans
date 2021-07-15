# Quarkus-959 - Improved Developer Joy with DevServices

JIRA link: https://issues.redhat.com/browse/QUARKUS-959

The goal is to verify proper functionality of the Quarkus DevServices

## Scope of the testing
Test development will focus on  
 - Extend CRUD test cases with applications that spin up the database dev services. Relational databases for which we have supported extensions:
   - PostgreSQL
   - MariaDB
   - MySQL
   - MSSQL
 - Cover "DB schema evolution" tools as Quarkus-flyway. If there is any conflict at the start-time, the error should be logged in a way that could understandable by the developer and fix it.
 - Extend messaging test cases with applications that spin up messaging dev services. Messaging services for which we have supported extensions:
   - AMQP: quarkus-smallrye-reactive-messaging-amq
   - Kafka: RedPanda
 - Use the existing dev mode test coverage to ensure that running scenarios without dev services is still possible
 - Cover DEV UI
 - Quarkus app is waiting for the necessary services to start, developer must be informed about started services progress


### Impact on testsuites and testing automation:
 - Enhance Quarkus QE test framework to cover all the features that are part of the scope
 - Add test coverage into the Quarkus QE test suite only for baremetal
 - Ensure this coverage works both in JVM and NATIVE mode 
 - Make sure the service is started just once and not respinned multiple times
 - The startup time will be increased, so must be clear from a developer’s point of view that Quarkus app is waiting for the necessary services to start. 
 - A developer must be able to run test with dev-services enabled and also run the same service in dev mode. Both services (test + dev-services and quarkus dev-mode + dev-services) must be not interfere with each other.
 - Developers must be able to develop his services following a continuos testing approach 
 - Ensure that the user is notified if there is no Docker installed
 - Run some tests in resources restricted environment. If the services are unable to boot (e.g. kafka) there should be some warning to the end users. 

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the features
 - Ensure good user experience and simplicity of use

## Contacts
* Tester: Pablo Gonzalez <pagonzal@redhat.com>