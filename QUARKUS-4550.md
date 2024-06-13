# QUARKUS-4550 - Enable proxy configuration for OpenTelemetry exporters

JIRA: https://issues.redhat.com/browse/QUARKUS-4550

Upstream issues: https://github.com/quarkusio/quarkus/issues/39519, https://github.com/quarkusio/quarkus/issues/39729

Upstream PRs: https://github.com/quarkusio/quarkus/pull/39543, https://github.com/quarkusio/quarkus/pull/39733

Quarks OpenTelemetry extension has builtin Vert.x-based OTLP exporter for gRPC and `http/protobuf` protocols.
Underneath Vert.x HTTP client is used that supports proxy options.
With this change, proxy options are now propagated when Quarkus-specific properties are set.
Additionally, when JDK's own HTTP / HTTPS proxy system properties are set, Quarkus respects them as well.

## Scope of the testing
What should we test:
- Proxy in native mode, JVM mode
- Proxy with username and password
- Proxy with `grpc` protocol
- Proxy configured via Quarkus-specific properties
- Proxy enabled / disabled during the runtime

What should we not test due to heedful resource management:
- Proxy in a DEV Mode as this change is about configuration properties propagation in a standard fashion (DEV mode or not)
- OpenShift scenario; deploying proxy in a different pod would allow us to test different hostname than `localhost`, however
  it would add additional complexity
- JDK-specific properties because upstream contains mapping test which ensures that properties are propagated to Vert.x.
  Then it works exactly same as Quarkus-specific properties. Therefore, I think it's costly overkill to test it.
- Proxy with `http/protobuf` as Jaeger does not support this option, and it would require using new Observability DEV Service.
  I have explored this option, and it would mean adding a new test with Micrometer OTLP exporter and fetching data from Grafana.
  Basically we don't need to test it because we only need to know that configuration properties are propagated to underlying
  Vert.x client correctly.
- Testing this feature in both `opentelemtry` and `opentelemetry-reactive` modules is unnecessary as this feature is implemented
  on lower level and is not specific to Quarkus REST or RESTEasy.
- Proxy without username and password because it would require us to run extra test to check there are no headers.
  This functionality is provided by Vert.x, and it's about setting Basic authorization header.
  Therefore, we just need to test whether headers are set.
  It's clear the headers will be missing if we don't set them


## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
We have OpenTelemetry modules (`opentelemetry` and `opentelemetry-reactive`) in our Quarkus QE Test Suite.
They cover scenarios where the proxy is disabled.
Quarkus upstream test coverage tests configuration mapping from JDK properties and from Quarkus properties to Vert.x.

### Impact on test suites and testing automation
New test classes will be added to the Quarkus QE Test Suite module `opentelemetry`.
This test class will be run in the JVM mode and in the native mode.
We will test all proxy options using gRPC protocol.

### Impact on resources
I expect roughly additional 6 seconds for `OpenTelemetryProxyIT` including start of Jaeger container.
This new test class has same build-time properties as existing `io.quarkus.ts.opentelemetry.OpenTelemetryIT` and does not require custom build.
Hence, additional test execution time for the native mode is roughly 10 seconds.

## Future considerations
- test proxy with `http/protobuf` protocol in a DEV mode using Observability Dev Services
- test proxy via secured channel (use HTTP and not HTTPS)
- test proxy with different hostname in OpenShift (proxy deployed in a different pod)

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [OTel TD - inspired by 3.8.5 backports #2](https://issues.redhat.com/browse/QQE-675)