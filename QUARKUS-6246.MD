# QUARKUS-6246 - Support customization of gRPC server building

JIRA: https://issues.redhat.com/browse/QUARKUS-6246

Upstream issues: https://github.com/quarkusio/quarkus/issues/45216

Upstream PR: https://github.com/quarkusio/quarkus/pull/47127

Quarkus 3.22 introduced a way to customize the gRPC server.
The feature is supported for both legacy and Vert.X-based gRPC servers.
Users can provide multiple customizers as CDI beans and order them by priority.
The customization is performed by user-defined `io.quarkus.grpc.api.ServerBuilderCustomizer` implementors.
The interface javadoc mentions that the API is experimental.

## Scope of the testing

New test coverage should verify that:

- Customizers are applied in a defined order
- Both the legacy gRPC server and the Vert.X-based gRPC server can be customized
- The feature works in native mode

## Existing test coverage

Upstream test coverage verifies in JVM and native mode that a single customizer provided as a CDI bean is invoked.
Which means that they do not verify scenarios where the user has defined multiple customizers and set their priority,
nor do they verify that the customizations are applied. OpenShift scenarios are not tested as well.

### Impact on test suites and testing automation

New test methods will be added to existing test classes in the `http/grpc` module of our Quarkus QE Test Suite.

### Impact on resources

Impact on resources will be negligible.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [gRPC class ServerBuilder](https://grpc.github.io/grpc-java/javadoc/io/grpc/ServerBuilder.html)
- [Quarkus QE Test Framework - gRPC is not supported on OpenShift](https://github.com/quarkus-qe/quarkus-test-framework/issues/1053)