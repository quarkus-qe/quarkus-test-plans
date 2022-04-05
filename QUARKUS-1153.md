# QUARKUS-1153 - code.quarkus.redhat.com - allow user select a version

JIRA link: https://issues.redhat.com/browse/QUARKUS-1153

Allow user to select version of Red Hat build of Quarkus using code.quarkus.redhat.com

It's not intended to provide a selection of concrete versions, but to provide the one supported in the stream.

## Scope of testing
- Exploratory testing of code.quarkus
- Test development for automated checks

## Getting familiar with the feature
- Exploratory testing of code.quarkus
- Generating applications from different streams
  - expected to have 2.2 and 2.7 streams  

## Automated test development
- Test development in `quarkus-startstop` test-suite to ensure stream selection option is available.
- Automated test development to ensure 2 streams are available is under consideration.
  - 2 streams are guaranteed only for certain time (2 months), see https://access.redhat.com/support/policy/updates/jboss_notes#p_quarkus  

## Impact on test suites and test environment
- Both upstream integration tests and planned 