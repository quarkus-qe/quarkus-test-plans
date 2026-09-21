# QUARKUS-7865 - Support arbitrary keystore/truststore types via 'other' config

JIRA: https://redhat.atlassian.net/browse/QUARKUS-7865

Upstream issue: https://github.com/quarkusio/quarkus/issues/50304

Upstream PR: https://github.com/quarkusio/quarkus/pull/53491

The TLS registry previously supported only PEM, PKCS12, and JKS. This feature adds an `other` configuration group to
`KeyStoreConfig` and `TrustStoreConfig`, so any keystore type can be configured without writing Java code. The driving
use case is Bouncy Castle FIPS (`BCFKS`), required by Keycloak.

Two loading strategies are supported:

* **Standard fallback** — `KeyStore.getInstance(type, provider)` loads the store from
  `quarkus.tls.key-store.other.path`, with `.password`, `.alias`, and `.alias-password`.
* **Custom factory** — a CDI bean implementing `KeyStoreFactory` or `TrustStoreFactory`, annotated
  `@Identifier("<type>")`, receives the full `OtherKeyStoreConfiguration` including arbitrary `params`. Covers types
  needing custom initialization, such as PKCS#11 tokens.

`other` is mutually exclusive with `pem`, `p12`, `jks`, and `KeyStoreProvider` beans. Reload works for both strategies.

## Scope of the testing

In scope:

* BCFKS mTLS end-to-end: server keystore and truststore via `other.type=BCFKS` / `other.provider=BCFIPS` in a named TLS
  bucket, verified with a real mTLS request. The client stays on a programmatically configured Vert.x `WebClient`, as
  upstream does — the feature under test is the server-side registry configuration.
* A custom `KeyStoreFactory` CDI bean resolved through `other.type` and exercised over a real HTTPS request.
* Certificate reload with the standard fallback: replace the keystore file on disk, trigger reload, confirm the new
  certificate is served over a live connection.

Out of scope:

* PKCS#11 / HSM — no hardware token in CI.
* Compatibility matrix across third-party security providers.
* Native mode for the BCFKS scenario — not supported upstream either. See [Bouncy Castle BCFIPS provider fails native executable build after the BC FIPS version bump to 1.0.2.4](https://github.com/quarkusio/quarkus/issues/37500) and [Disable Quarkus - Integration Tests - Bouncy Castle FIPS JSSE module in native mode](https://github.com/quarkusio/quarkus/pull/47510). The BCFIPS provider is required to load the BCFKS keystore.

Modes: JVM and native (non-BCFIPS scenarios only).

## Extension interactions and integration points

The TLS registry is cross-cutting: any extension accepting `tls-configuration-name` (REST Client, gRPC, Vert.x,
Messaging) can consume an `other`-backed bucket with no code change. The risk is not per-extension wiring but whether
an `other`-typed bucket yields usable Vert.x `KeyCertOptions` at all, so every scenario drives it through a real HTTP
interaction instead of asserting on registry state.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:

* Reviewed the upstream issue and pull request.
* Reviewed `OtherKeyStoreConfig`, `OtherKeyStores`, `CertificateRecorder`, `KeyStoreFactory`, and `TrustStoreFactory`.
* Reviewed the upstream integration test `bouncycastle-fips-jsse` and the TLS registry guide.
* Reviewed existing QE coverage in `security/bouncycastle-fips`, `security/https`, and `http/rest-client-reactive`.

## Existing test coverage

Upstream `extensions/tls-registry` tests inspect the `TlsConfigurationRegistry` bean directly — no HTTP connection is
made and native mode is not exercised:

* `DefaultOtherKeyStoreTest`, `DefaultOtherKeyStoreWithAliasTest`, `NamedOtherKeyStoreTest`,
  `DefaultOtherTrustStoreTest` — standard fallback.
* `OtherKeyStoreWithFactoryTest`, `OtherTrustStoreWithFactoryTest`, `OtherKeyStoreFactoryWithParamsTest` — factory path.
* `ReloadOther{Key,Trust}Store[WithFactory]Test` — reload for both strategies.
* `Other{Key,Trust}StoreCredentialsProviderTest` — credential provider.
* `TooMany{Key,Trust}StoreConfigured*AndOtherTest` — mutual exclusivity.

`integration-tests/bouncycastle-fips-jsse` is the exception: it configures server keystore and truststore via
`other.type=BCFKS` and makes a real mTLS request from a Vert.x `WebClient`.

In the QE suite:

* `security/bouncycastle-fips/bcfips` — only checks that the BCFIPS provider is registered.
* `security/bouncycastle-fips/bcFipsJsse` — already performs a BCFKS mTLS request, but configures the server with the
  legacy `quarkus.http.ssl.certificate.key-store-file-type` / `key-store-provider` properties and a custom
  `SecretProvider`, not the TLS registry. Its native build is skipped (`quarkus.build.skip=true`) pending
  [#37500](https://github.com/quarkusio/quarkus/issues/37500).
* `security/https/TlsRegistryCertificateReloadingIT` — reload for PEM only.
* `http/rest-client-reactive/TLSRegistryIT` — TLS registry with PEM and PKCS12.

So the `other` group is not exercised anywhere in the QE suite, and neither is `KeyStoreFactory` / `TrustStoreFactory`.
The closest precedent is `InMemoryClientKeyStoreProvider` in `http/rest-client-reactive`, which implements the older
`KeyStoreProvider` SPI over a live connection.

## Planned coverage

1. **BCFKS mTLS through the TLS registry** (`security/bouncycastle-fips/bcFipsJsse`) — duplicate the server from the 
  legacy `quarkus.http.ssl.certificate.*` properties to `quarkus.tls.key-store.other.*` (unnamed), 
  `quarkus.tls.<bucket>.key-store.other.*` (named) and the matching truststore, under a unnamed or named bucket referenced 
  by `quarkus.http.tls-configuration-name`; the existing mTLS request must still succeed. The legacy variant is kept 
  alongside so the migration does not drop coverage of the old properties. The module already ships the BCFKS stores and 
  the BCFIPS/BCJSSE providers.

2. **Custom `KeyStoreFactory` over HTTPS** (`security/https`) — a `KeyStoreFactory` bean for a synthetic type
  identifier, selected through `other.type`, serving a real HTTPS request. Upstream only asserts the resulting registry 
  object.

3. **Reload with the standard fallback over a live connection** (`security/https`) — an `other`-typed keystore backed
  by a PKCS12 file, regenerated on disk and reloaded, following `TlsRegistryCertificateReloadingIT`. Upstream only 
  inspects the registry after `reload()`.

## Impact on test suites and testing automation

* No new modules and no new CI jobs; `security/bouncycastle-fips` and `security/https` are already part of the build 
  `quarkus-test-framework` cannot generate `other` configuration: `Certificate.Format` offers only PEM, ENCRYPTED_PEM, 
  JKS, and PKCS12, and `CertificateBuilder` emits `key-store.{pem,p12,jks}.*`. Scenario 1 therefore reuses the BCFKS 
  stores already committed in the module, and scenario 3 uses `@Certificate(configureKeystore = false)` so the
  generated PKCS12 file is reused without the conflicting `p12` properties — otherwise both groups are set and startup
  fails on the mutual-exclusivity check. Extending the framework with an `other` format is not proposed, as it would
  need a certificate generator per arbitrary keystore type.

* Scenario 1 depends on `quarkus.security.security-providers=BCFIPSJSSE`, already set in the module.

## Impact on resources

The added tests should have **low impact** on resources:

* Small execution time increase (around 2 to 3 m)
* Tests will be executed on baremetal and OpenShift in JVM and native mode.

## Contacts

* Tester: Roberto Cortez <radcortez@ibm.com>

## References

* https://quarkus.io/guides/tls-registry-reference#other-keystore-types
