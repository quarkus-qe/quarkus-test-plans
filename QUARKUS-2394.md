# QUARKUS-2394 Drop OpenTelemetry Jaeger exporter

Jira: https://issues.redhat.com/browse/QUARKUS-2394

OpenTelemetry Jaeger exporter has tech-preview support in RHBQ 2.7. Starting with RHBQ 2.13 the tech-preview support is dropped. The extension `quarkus-opentelemetry-exporter-jaeger` and `quarkus-opentelemetry-exporter-otlp` are planned to be moved to Quarkiverse in the future, `quarkus-opentelemetry` extension is the one to be used instead of Jaeger exporter.

## Scope of the testing

All references to `quarkus-opentelemetry-exporter-jaeger` and `quarkus-opentelemetry-exporter-otlp` are going to be removed. Current coverage is going to be replaced by `quarkus-opentelemetry`. The extension `quarkus-opentelemetry` will integrate `quarkus-opentelemetry-exporter-otlp` and set it as the default exporter.

Additionally we need to be sure that all metadata related to `quarkus-opentelemetry-exporter-jaeger` and `quarkus-opentelemetry-exporter-otlp` are dropped especially in https://code.quarkus.redhat.com/?extension-search=origin:platform%20quarkus-opentelemetry-exporter-jaeger (to be checked on the stage instance)

### Impact on test suites and testing automation

The main impact is related to scenarios configuration and documentation in the following test suites:
- Quarkus Test Suite
- Beefy Scenarios
- Start&Stop
- Marete


### Impact on resources:

- No additional requirements for resources in lab. Current coverage is going to be covered by `quarkus-opentelemetry` and the required resources are the same.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use
- Review upstream documentation

## Advanced topics for test development

- No advanced topics found

## Contacts

* Tester: Pablo Gonzalez <pagonzal@redhat.com>

## References

- [Quarkus OpenTelemetry guide](https://quarkus.io/guides/opentelemetry)
