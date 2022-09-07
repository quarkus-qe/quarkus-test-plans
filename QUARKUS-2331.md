# QUARKUS-2331 Promote the Kubernetes Config extension from TP to full support

Jira: https://issues.redhat.com/browse/QUARKUS-2331

This feature is about productization of the Quarkus Kubernetes Config extension. Our test coverage must ensure the extension
is product ready and all features work as expected. Currently, the Quarkus QE Test Suite covers following topics:

1. Properties retrieved from ConfigMaps and Secrets that contain literal data.
2. Properties retrieved from ConfigMaps and Secrets created from an `application.properties` file.
3. Automatically generated OpenShift resources that grant permissions required to access Secrets/ConfigMaps (including role binding enabled by `quarkus.kubernetes-config.secrets.enabled=true` property).
4. ConfigMaps/Secrets update is reflected on application reboot.

The Quarkus sufficiently test isolated functionalities (unit tests in a runtime module of the extension) and runs integration
tests as part of the `quarkus-integration-test-kubernetes-standard` project, however these tests only runs against mocks.

## Scope of the testing

Following topics should be newly covered:

1. Properties retrieved from ConfigMaps and Secrets created from an `application.yml` file.
2. Priority of obtained properties:
   1. Properties defined in Secrets have higher priority than same properties defined in Config Maps.
   2. When multiple ConfigMaps (or Secrets) are used, ConfigMaps (or Secrets) defined later in the list have a higher priority that ConfigMaps defined earlier in the list.
   3. The properties obtained from the ConfigMaps and Secrets have a higher priority than (i.e. they override) any properties of the same name that are found in application.properties (or the YAML equivalents), but they have lower priority than properties set via Environment Variables or Java System Properties.
3. If property `quarkus.kubernetes-config.fail-on-missing-config` set to true, the application will not start if any of the configured config sources cannot be located.
4. Explicitly defined namespace to look for config maps and secrets. Namespace should differ from the namespace used to run the application.

### Impact on test suites and testing automation

* Two integration tests should be added:
  * Test that application will not start when config source cannot be located.
  * One that test priority of obtained properties. We can define the properties in a YAML file and another namespace as this fact should not affect the prioritization.

### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be about 4 minutes per each integration test (application started in OpenShift respectively) as currently we have 4 integration tests that run roughly 17 minutes

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Advanced topics for test development

- No advanced topics found

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Promote the Kubernetes Config extension from TP to full support](https://issues.redhat.com/browse/QUARKUS-2331)
- [Kubernetes Config](https://quarkus.io/guides/kubernetes-config)