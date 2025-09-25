# QUARKUS-6397 - Show the support status for the chosen product in CLI tooling

JIRA: https://issues.redhat.com/browse/QUARKUS-6397

Show the support status for the chosen product in CLI tooling

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/49589
- https://github.com/quarkusio/quarkus/pull/49487

Documentation:
- https://quarkus.io/guides/extension-metadata#extension-offerings
- https://quarkus.io/guides/extension-registry-user#limiting-extension-catalog-to-an-offering

## Scope of testing

This feature is part of [QUARKUS-6292 (Extend the platform with offering)](https://issues.redhat.com/browse/QUARKUS-6292) feature.
You can find the test plan at https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6292.md.

The Quarkus CLI feature is defined in https://issues.redhat.com/browse/QUARKUS-6397.
The Quarkus CLI should be able to show which extension is part of which offering, when listing the extension.
Documentation about definition and usage can be found in references section of this test plan.

At the moment of Quarkus 3.27 the only way to add the offering is to add it to `.quarkus/config.yaml` manually.
There is plan to add the offering using the Quarkus CLI command (https://github.com/quarkusio/quarkus/issues/49090).


## Automated test development

Quarkus CLI tests will cover the added `support-scope` option when listing the available extensions.
These test will be added to Quarkus QE test suite in `quarkus-cli` module
The created test will these cases:
- No offering is selected
- IBM offering is selected
- RedHat offering is selected

These tests will be executed only with product testing not as part of the daily runs, as they need to be run against product registry.
At the moment there is no plan to have multiple registries for each offering so the tests can run against same registry.

In addition, the test to check if `support-scope` if working will be added to `QuarkusCliExtensionsIT` to ensure executing this argument is not causing errors.
This test will be executed as part of the daily testing.


### Impact on TS and resources
The test will be run only on baremetal in JVM mode.
Total execution time increase should be in a few minutes maximum.


## References
- https://quarkus.io/guides/extension-metadata#extension-offerings
- https://quarkus.io/guides/extension-registry-user#limiting-extension-catalog-to-an-offering
- https://issues.redhat.com/browse/QUARKUS-6397

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
