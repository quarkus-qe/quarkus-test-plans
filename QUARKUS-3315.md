# QUARKUS-3315 - Productise quarkus-jms-spi-deployment artifact

JIRA: https://issues.redhat.com/browse/QUARKUS-3315

CEQ team is planning to have a full support of pooling and transaction integration with JMS client (QPid, Artemis, IBM MQ) by using quarkus-pooled-jms to simplify migration of older customers Fuse customers to CEQ.

## Scope of the testing
 * RHBQ QE team will ensure availability of the quarkus-jms-spi-deployment artifact
 * Functional testing is on CEQ side

### Impact on test suites and testing automation
 * Marete checks for maven repo zip will be extended to ensure availability of the quarkus-jms-spi-deployment artifact

### Impact on resources
No need for additional resources.

## Contacts
* Tester: Rostislav Svoboda <rsvoboda@redhat.com>
