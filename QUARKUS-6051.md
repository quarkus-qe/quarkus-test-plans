# QUARKUS-6051 - JSON option support for named logging handlers

JIRA: https://redhat.atlassian.net/browse/QUARKUS-6051

Upstream PR: https://github.com/quarkusio/quarkus/pull/53258

This feature adds support for configuring JSON logging independently for named logging handlers. Previously, if users wanted a named logging handler to produce JSON output, it had to be enabled globally and the named handlers could not override the global JSON logging configuration. With this new feature the named handlers can explicitly enable or disable JSON formatting regardless of the global setting.

## Scope of the testing
Verify that JSON logging can be configured independently for named logging handlers.

For each named handler type — **console**, **file**, **syslog**, and **socket** — verify both override scenarios:

* **Global JSON off, handler JSON on:** the handler produces JSON output.
* **Global JSON on, handler JSON off:** the handler produces plain text output.

Additional verification per handler:
* **File handler:** the configured log file is created in both cases.
* **Syslog handler:** output is sent to the configured syslog receiver.
* **Socket handler:** output is sent to the configured TCP receiver.

## Existing test coverage
Upstream tests for named console, file, socket and syslog handlers by checking the correct formatter is assigned.

The QE coverage will test the feature from a user perspective. The tests will start a Quarkus application, trigger logging through a REST endpoint and verify that the named handler produces the expected JSON or plain text.

The QE test suite contains tests for logging, but does not currently verify JSON configuration for named logging handlers.

### Impact on test suites and testing automation

* Console and File tests will be added under the `logging/jboss` module.
* Syslog and Socket tests will be added under the `logging/thirdparty` module.

### Impact on resources

The added tests should have **low impact** on resources:

* Estimated execution time increase should only be a few minutes.
* Tests will be executed on baremetal in JVM and native mode. 

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed upstream issue and pull request.
* Reviewed documentation on logging.

## Contacts

* Tester: Andy Yan <andy.yan@ibm.com>

## References
* https://quarkus.io/guides/logging#console-log-handler
* https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.19#other-changes-gear-white_check_mark 
* https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.26#enable--enabled-in-configuration-properties-gear-white_check_mark

