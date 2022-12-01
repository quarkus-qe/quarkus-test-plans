# QUARKUS-2524 - Make the list of productized dependencies a part of product testing deliverables

JIRA link: https://issues.redhat.com/browse/QUARKUS-2524

Make the list of productized dependencies a part of product testing deliverables, related to [QUARKUS-2072 test plan](QUARKUS-2072.md).

## Scope of the testing
- Agree on the location and file name with productization team
- Ensure the file gets generated and delivered
- Automate checks for expected deliverables presence

### Impact on test suites and testing automation
- Develop a new internal test suite to check for the presence of expected deliverables.
- Prepare testing automation and job definition
- Include the checks in the acceptance pipeline

## Advanced topics for test development
N/A