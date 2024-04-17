# QUARKUS-3461 Verify behaviour and functionality of Quarkus CLI update command with RHBQ
- JIRA link: https://issues.redhat.com/browse/QUARKUS-3461
- Quarkus guides:
    - https://quarkus.io/guides/update-quarkus

## Scope of the testing
- Exploratory testing of the feature over supported versions of RHBQ and Quarkus upstream
- Writing test coverage for this feature is continual work, requires update for every release of Quarkus
- Test update between each of Quarkus LTS release versions
- Test update of multi-module projects
- Test coverage in QQE TS:
  - Test most frequently applied OpenRewrite recipes:
    - org.openrewrite.properties.ChangePropertyKey
    - org.openrewrite.quarkus.ChangeQuarkusPropertyKey
    - org.openrewrite.java.ChangeMethodName
    - org.openrewrite.java.dependencies.ChangeDependency
    - org.openrewrite.maven.UpgradePluginVersion
    - org.openrewrite.maven.UpgradeDependencyVersion
    - org.openrewrite.quarkus.AddQuarkusProperty
    - org.openrewrite.quarkus.DeleteQuarkusProperty
    - org.openrewrite.maven.AddDependency
  - Dynamically create Quarkus applications and manually copy test classes from test classpath. Each application should contain a group of dependencies and properties that were modified in the [recipes](https://github.com/quarkusio/quarkus-updates/tree/main/recipes/src/main/resources/quarkus-updates/core). Multi-module projects can only be created manually
  - Created apps should reflect the customer's usage scenario including test coverage  
  - Test that initial application was successfully built. Update application to selected stream and verify that app was successfully run, test expected changes based on [recipes](https://github.com/quarkusio/quarkus-updates/tree/main/recipes/src/main/resources/quarkus-updates/core) and [Quarkus migration guides](https://github.com/quarkusio/quarkus/wiki/Migration-Guides)

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Existing test coverage
- [Quarkus updates](https://github.com/quarkusio/quarkus-updates/tree/main/recipes-tests/src/test) 

### Impact on test suites and testing automation
- New tests will be added to [quarkus-cli](https://github.com/quarkus-qe/quarkus-test-suite/tree/main/quarkus-cli) module in QQE TS.

### Impact on resources
- Tests in QE Test Suite increase execution time up to 10 minutes in JVM. In case of testing application in native mode in future, this will extend execution time too.

## Contacts
- Tester: Georgii Troitskii <gtroitsk@redhat.com>