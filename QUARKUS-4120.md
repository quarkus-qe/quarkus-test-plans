# Quarkus 4120 - Count kafka messages send to dead-letter queue in micrometer metrics

JIRA link: https://issues.redhat.com/browse/QUARKUS-4120

## Scope of the testing
Verify that [issue 2473](https://github.com/smallrye/smallrye-reactive-messaging/issues/2473) si fixed.
Requires:
 - Setting up application with kafka connector and micrometer.
 - Forcing kafka to send messages to dead-letter-queue.
 - Confirm that these messages are counter by the micrometer.

## Automated test development
Quarkus test suite already contains tests for [kafka and micrometer](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/monitoring/micrometer-prometheus-kafka).
These will be enhanced to support testing of this issue.
This will require new test class, which will lead to new application build during testing

## Impact on resources
Additional ~1.5 minutes of test execution time on JVM, 5 minutes on native.

# Links
Issue link: https://github.com/smallrye/smallrye-reactive-messaging/issues/2473
