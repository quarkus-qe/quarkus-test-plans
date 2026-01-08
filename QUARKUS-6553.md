# QUARKUS-6553 Properly capture Quarkus information for JFR events

JIRA: https://issues.redhat.com/browse/QUARKUS-6553

Properly capture Quarkus information for JFR events

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/49302

Documentation:
- https://quarkus.io/guides/jfr#runtime-event

## Scope of testing
The QUARKUS-6553 is follow up on QUARKUS-6552, which is described in the test plan [here](https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6552.md).
The QUARKUS-6553 moving the JFR captured data from build time to runtime for application information.
Moved information are image mode (JVM/Native) and profiles.


## Automated test development
To ensure that the image mode and profiles are captured in runtime the new test will be added.
This test will use `quarkus.profile` property to set multiple profiles and the profiles will be compared with captured data.
By default `quarkus.profile` contains only one profile (prod, test or dev).

The image mode will be checked primary for native mode as it have 2 possible value (`NATIVE_RUN`, `NATIVE_BUILD`).
The value of the native mode will be checked to see if it equals `NATIVE_RUN`.
The JVM mode have same value for build time and runtime. 


### Impact on TS and resources
The test will be added to [`monitoring/jfr`](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/monitoring/jfr) module as part of the QUARKUS-6552 coverage.

The tests will be executed only on bare-metal for both JVM and native modes.
The expected time increase will be in few seconds as only one test is added.

The Semeru JDK will be excluded as it's not fully support the JFR yet.

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>
