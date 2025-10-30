# QUARKUS-6752 Testing and Verification of Semeru CE 21 & 17 on x86_64 (RHEL 8)

JIRA: https://issues.redhat.com/browse/QUARKUS-6752

Upstream issue: https://github.com/quarkusio/quarkus/issues/50570

Semeru JDK is going to be supported for IBQ 3.27 on RHEL x86_64.
Semeru JDK is an IBM's release of JDK, using OpenJDK's libraries and IBM's OpenJ9 JVM [[1]](https://developer.ibm.com/languages/semeru-runtimes/).
We're going to integrate Semeru JDK into quarkus-qe testing infrastructure and executing test suites with it.
Test suites themselves should require little to no changes.
Semeru JDK comes in two editions: Certified edition (CE) and Open Edition (OE).
These two differ only in licence.

In this instance we will use Semeru JDK 21 in OE or CE (whatever will be more feasible to use). 

## Infrastructure changes
Semeru JDK will need to be installed into shared QA tools with java versions.

Semeru JDK will need to be added to Jenkins Java enum.
For testing, new jenkins jobs will be created, using just different JDK.

## Scope of testing
Initial focus will be on quarkus 3.27, which will be supported with this JDK.
Also jobs for quarkus main will be added.

Testsuites to be tested:
- Quarkus QE Test Suite - baremetal
- Startstop special chars
- Startstop code.quarkus
- Quickstarts
- AI samples

## Impact on test suites
In ideal case, test suites should require no changes at all.

## Impact on resources
New jenkins jobs will be created, for each tested test suite.
Expected execution time will be similar as in other cases for these TS:
- Quarkus QE Test Suite - baremetal - 2h
  - This time is average for entire module, if tests run in parallel. In sequence those tests would take about 12 hours.
- StartStop special chars - 15m
- StartStop code quarkus - 1h 
- Quickstarts - 1h
- AI samples - 15m


## Contacts
* Tester: Martin Ocenas <mocenas@redhat.com>

## References:
[1] - https://developer.ibm.com/languages/semeru-runtimes/
[2] - https://developer.ibm.com/languages/java/semeru-runtimes/downloads/
