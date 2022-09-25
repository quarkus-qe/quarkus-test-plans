# QUARKUS-2334 Define how continuous testing of upstream documentation can be implemented

Jira: https://issues.redhat.com/browse/QUARKUS-2334

The goal of this feature is to reduce duplication of effort with Quarkus documentation so that upstream documentation
are of quality sufficient to be easily convertible to product docs.

## Scope of the testing
This feature is about testing the documentation team proof of concept for one source documentation, as well as our
proposed process.

A standalone Jira, linked to this issue will be open by the documentation team to track the effort on the concrete 
piece of documentation that will be done with this PoC.

All new future documentation books will require their own Jira. Features requiring extensive documentation should have
their own documentation feature request to ensure that the new documentation content is checked while it is developed 
upstream.

### Impact on test suites and testing automation
* A style check implemented through grammar lint should be added to merge request actions on upstream Quarkus docs.
* Upstream documentation for Quarkus will require automated preview rendering so that resulting documentation book can 
  be reviewed in both quarkus.io site format and approximation of product documentation format. This is tracked by
  [quarkusio/quarkus#28087](https://github.com/quarkusio/quarkus/issues/28087).

### Impact on resources
* No impact on hardware resources in QE labs.
* Preview automation may take up capacity on `quarkusio` team GitHub runners.
* So far, the planned QE allocation by product team is 10-15% of capacity of one QE per month. This capacity will be 
  taken from other assignments.

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature epic: [QUARKUS-QUARKUS-2334 Define how continuous testing of upstream documentation can be implemented](https://issues.redhat.com/browse/QUARKUS-2334)
* [quarkusio/quarkus Introduce preview of documentation changes for PRs #28087](https://github.com/quarkusio/quarkus/issues/28087)