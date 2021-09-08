# QUARKUS-1255 Dev support for JaCoCo extension
 - JIRA link: https://issues.redhat.com/browse/QUARKUS-976
 - Quarkus Guide: https://quarkus.io/guides/tests-with-coverage
 - Release blog post: https://quarkus.io/blog/quarkus-1-13-0-final-released/#test-coverage-reports

## Scope of the testing
 - Focus on exploratory testing of the feature.
 - Extension has no impact on performance in production.
 - Small impact on test execution time as JaCoCo instruments classes to capture the coverage. On the other hand the same impact would be noted when JaCoCo is used directly, quarkus-jacoco extension makes user experience much nicer.
 - No impact for compatibility, extension is not used in application code.

## Getting familiar with the feature
 - Focus on exploratory testing of the feature
 - Experiment with the extension using single module maven project generated using code.quarkus
 - Experiment with the extension using several modules in Quarkus integration tests
 - Experiment with the extension using standalone Maven project with several modules
   - https://github.com/rsvoboda/multi-module-jacoco/tree/multiple-app-modules
 - Experiment with the extension using standalone Maven project with several test modules
   - https://github.com/rsvoboda/multi-module-jacoco/commit/ff78b19e88abd94df438d0e3566d89c784aabc23
 - Get aggregated report from all the modules available in Maven project
   - https://github.com/rsvoboda/multi-module-jacoco/commit/fb07ba5881d066d92ac1c94c2ea924020d4829e7
 
## Automated test development
 - New test to be added into Quarkus QE tests suite to cover scenario with `quarkus-jacoco` extension

## Advanced topics for test development
The are no advanced topics to be checked as the extension is considered for Development support and its complexity is not high.
