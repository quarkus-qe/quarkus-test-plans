# QUARKUS-5709 - Give enable_recovery more granularity

JIRA: https://issues.redhat.com/browse/QUARKUS-5709

Upstream issues: https://github.com/quarkusio/quarkus/issues/47117

Upstream PRs: https://github.com/quarkusio/quarkus/pull/47118, https://github.com/quarkusio/quarkus/pull/47067

The Agroal version 2.6 allows users to configure for individual XA connections whether the transaction recovery is enabled or disabled.
Before Quarkus 3.22, users could only configure the recovery for all the XA connections with the `quarkus.transaction-manager.enable-recovery` configuration property.

## Scope of the testing

Verify that the transaction recovery can be disabled for one specific datasource when the global transaction recovery is enabled.

## Existing test coverage

Upstream test coverage has one test using JVM mode that verifies the recovery is disabled when two data sources are configured, one has the recovery enabled, and one has the recovery disabled. Both data sources use the same H2 instance.
Quarkus QE has comprehensive test coverage for the transaction recovery in JVM mode, native mode, and in OpenShift. Our tests only pass if the transaction recovery is disabled after the restart.
The upstream test coverage verifies a scenario where one datasource has the recovery enabled and one disabled; however, it verifies whether the connection has enabled autocommit, while our tests actually check whether recovery happens or does not happen.

### Impact on test suites and testing automation

The `MariaDbTransactionGeneralUsageIT` test class will be altered to disable recovery using the newly introduced configuration property instead of the `quarkus.transaction-manager.enable-recovery` used by other test classes.

### Impact on resources

No impact on resources.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Narayana transaction manager integration](https://quarkus.io/guides/datasource#narayana-transaction-manager-integration)
