# QUARKUS-4592 - Enhance TLS support

JIRA: https://issues.redhat.com/browse/QUARKUS-4592

Upstream Working Group: https://github.com/orgs/quarkusio/projects/24/views/1

Upstream issues:
- https://github.com/quarkusio/quarkus/issues/34193 (epic)
- https://github.com/quarkusio/quarkus/issues/41000
- https://github.com/quarkusio/quarkus/issues/41001
- https://github.com/quarkusio/quarkus/issues/41002
- https://github.com/quarkusio/quarkus/issues/41003
- https://github.com/quarkusio/quarkus/issues/41004
- https://github.com/quarkusio/quarkus/issues/41005
- https://github.com/quarkusio/quarkus/issues/41006
- https://github.com/quarkusio/quarkus/issues/41009
- https://github.com/quarkusio/quarkus/issues/41010
- https://github.com/quarkusio/quarkus/issues/41042
- https://github.com/quarkusio/quarkus/issues/41503
- https://github.com/quarkusio/quarkus/issues/41504
- https://github.com/quarkusio/quarkus/issues/41548
- https://github.com/quarkusio/quarkus/issues/42050

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/40990
- https://github.com/quarkusio/quarkus/pull/41030
- https://github.com/quarkusio/quarkus/pull/41048
- https://github.com/quarkusio/quarkus/pull/41095
- https://github.com/quarkusio/quarkus/pull/41126
- https://github.com/quarkusio/quarkus/pull/41153
- https://github.com/quarkusio/quarkus/pull/41418
- https://github.com/quarkusio/quarkus/pull/41501
- https://github.com/quarkusio/quarkus/pull/41573
- https://github.com/quarkusio/quarkus/pull/41665
- https://github.com/quarkusio/quarkus/pull/42042
- https://github.com/quarkusio/quarkus/pull/42082
- https://github.com/quarkusio/quarkus/pull/42105
- https://github.com/quarkusio/quarkus/pull/42134

This ticket concerns general effort to improve and unify TLS support in Quarkus core and across Quarkus extensions.
User experience is bettered with integration to Quarkus CLI that allows to generate PKCS12 certificate, its keystore, truststore.
It's also possible to generate certificate authority for development purposes only or self-sign certificates.
Quarkus application is then using these certificates in DEV mode as the CLI configures the app via environment file.
Major effort is to have one TLS registry for TLS configurations. Either named or default.
Extensions then can access configuration from CDI container or respective build item.
From what I can see, the old ways to configure TLS in extensions are still valid and for now, the TLS registry is added as additional option.
User-wise, apart from having to configure TLS just once, the biggest change is periodic certificate reloading, 
Let's Encrypt support and OpenShift documentation explaining how to configure the Deployment.

What has changed in Quarkus extensions so far (remember this is living organism):
- OTel OTLP span exporter allows to communicate with an OTLP collector via secured channel
- OIDC global `trust all` flag can be configured from default TLS registry
- SmallRye Reactive Messaging allows to configure TLS with the registry in addition to the old ways
- Quarkus REST client has configuration property for named configuration bucket in addition to the old ways
- AMQ / MQTT / Pulsar and RabbitMQ are also configurable from the registry
- WebSockets NEXT integration is also finished (configures underlying HTTP client options under Vert.x WS)
- gRPC extension also propagates TLS configuration from the registry to underlying Vert.x client for the gRPC clients

## Scope of the testing
Main focus is on testing features documented in the TLS registry reference. That is:
- configure HTTP server to use HTTPS protocol
- configure management interface to use HTTPS protocol
- configure mTLS
- configure client to use HTTPS protocol; here we should concentrate on:
  - gRPC
  - Quarkus REST client
- accessing TLS registry API from CDI in application code (both named and the default one)
- configure keystore, truststore, cipher suits and protocol
- use PEM / PKCS12 keystore and truststore
- test certificate reloading in DEV mode
- use OpenShift serving certificates to generate and renew TLS certificates automatically
- generate certificates and DEV CA with Quarkus CLI, then run app and make sure the certs are configured and valid

Additionally, secured communication between Quarkus application and a Strimzi server must be tested.
We want to test registry where we already test extension-specific TLS configuration, 
but test both in all cases is not possible due to time limitations.
My plan is to split coverage between both options.
For example, secured communication with AMQ broker and OTLP collector will be tested if time allows, or delayed (with opened Quarkus QE Test Suite issue).

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
Quarkus unit test coverage is sufficient. As much as I am thinking how much do we need to test.
Integration tests are spare and sometimes missing, for example I didn't find OTel integration tests.
Quarkus QE Test Suite sufficiently covers TLS configuration for HTTP server, management interface, Kafka.
Smoke test TLS coverage for REST clients and gRPC is also present.

### Impact on test suites and testing automation
I am very reluctant to mirror all TLS tests for both extension-specific and TLS registry.
Using the TLS registry extension will be our primary option.
In general, my plan is to split test coverage between both and prioritize the registry when we cannot have both.
Several new test classes will be added starting integrations (e.g. Kafka), DEV mode tests that require reload and waiting period (before new certs are detected).
New OpenShift tests are required to test integration with serving certificates.

### Impact on resources
Counting in OpenShift, native, JVM and DEV mode, less than 45 minutes will be added to the test execution.

## Future considerations
- [OIDC integration](https://github.com/quarkusio/quarkus/issues/41001) is yet to be implemented
- [Reactive JDBC clients integration for DB2 and MSSQL extensions](https://github.com/quarkusio/quarkus/issues/41003)
- Let’s Encrypt support (I don't want our tests to call external authority for now due to network connectivity demands and stability of tests)
  For now, we shall test support locally as part of the ticket verification.
- WebSockets NEXT integration should be done together with WS Next product support
- Support for the TLS registry to the Mailer extension should be done when dedicated Mailer module is introduced to the QE Test Suite

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [TLS registry reference](https://quarkus.io/version/main/guides/tls-registry-reference)
- [Quarkus QE tracker](https://issues.redhat.com/browse/QQE-830)