# QUARKUS-8958 - AMQP 1.0 Claim-based Security (CBS) Implementation

JIRA: https://redhat.atlassian.net/browse/QUARKUS-8958

Upstream PR (SmallRye Reactive Messaging): https://github.com/smallrye/smallrye-reactive-messaging/pull/3415

Upstream docs (SmallRye Reactive Messaging): https://smallrye.io/smallrye-reactive-messaging/latest/amqp/cbs-authentication/

Azure Service Bus AMQP guide: https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-amqp-protocol-guide

Documentation: https://quarkus.io/guides/amqp-reference

## Feature summary

The SmallRye Reactive Messaging AMQP 1.0 connector gains support for Claim-Based Security (CBS), a token-based authorization scheme layered on top of an established AMQP connection. Instead of authenticating with username/password at the SASL layer, the client obtains a security token (typically a JWT or a SAS token), pushes it to the broker's CBS management node, and periodically refreshes it before expiry. The application must supply a `CbsTokenProvider` CDI bean (required by the default token manager) that returns a `CbsToken` when authenticating. The extension (`quarkus-messaging-amqp`) is fully supported (stable).

## Scope of the testing

In scope:

* **`set-token` exchange** against a CBS node (the Proton mock — see open questions) - connection authorized, send/receive works, and an unauthorized/expired token is rejected with a clear error.
* **`put-token` exchange** - the Azure Service Bus mechanism, exercised against Azure Service Bus and/or a request-reply CBS mock.
* **`CbsTokenProvider` usability** — writing a *meaningful* CDI bean that produces a real token asynchronously via `Uni`, including the case where token acquisition fails (the `Uni` fails) and the connection surfaces a clear, actionable error.
* **Automatic token refresh** - a short-lived token with `refreshAt()` before `expiresAt()`; verify the connection stays alive across a refresh and messaging continues uninterrupted. Refresh is always on for a CBS channel (see scenario 2) and is the behaviour most likely to regress silently.
* **Custom base token manager** - `cbs.token-manager` selects the *base* manager bean (default `default-cbs-token-manager` / `DefaultCbsTokenManager`, a one-shot authorize); verify a custom base manager is picked up. This does **not** toggle refresh.
* **Negative / failure modes** - `cbs.enabled=true` with no `CbsTokenProvider` bean present, an invalid `cbs.exchange` value, and a provider returning an expired token.
* **Both incoming and outgoing channels** authorized through CBS.

Target modes: JVM and native.

Target environments: **baremetal only** — the CBS scenarios need the in-process Proton mock (not reachable from OpenShift; see open questions), and the Azure smoke test is credential-gated.

## Extension interactions and integration points

* **CDI (ArC)** - the whole contract hinges on a user-supplied CDI bean that implements `CbsTokenProvider`. Native mode must retain the bean and the reflective wiring; a native run is mandatory for this reason.
* **Token providers / security extensions** - a realistic `CbsTokenProvider` typically calls out to an identity service. Worth exercising the provider using `quarkus-rest-client` (or OIDC client) to fetch a token, since that is how users will actually build one, and it crosses the async (`Uni`) boundary.
* **TLS registry** - CBS runs over an already-established (usually TLS) AMQP connection; confirm CBS composes with `quarkus.tls.*` / `AmqpClientOptions` TLS configuration rather than conflicting with it.
* **Observability** - verify failed authorization and refresh failures are observable (clear log messages at minimum) rather than surfacing only as a later dropped connection.

## Getting familiar with the feature

* Reviewed the upstream SmallRye Reactive Messaging PR [#3415](https://github.com/smallrye/smallrye-reactive-messaging/pull/3415) and its tests.
* Reviewed existing QE messaging/AMQP coverage and related plans
  [QUARKUS-1409](QUARKUS-1409.md) and [QUARKUS-959](QUARKUS-959.md).
* Reviewed the Quarkus AMQP extension integration test (`integration-tests/reactive-messaging-amqp`) and the AMQP Dev Services to determine which brokers can actually serve a CBS node for integration testing (see open questions).

## Existing test coverage

Upstream (smallrye-reactive-messaging) adds `AmqpCbsSetTokenTest`, `AmqpCbsPutTokenTest`, `AzureCbsTokenTest`, `AmqpDirectRequestReplyTest` and `AmqpRabbitMQDirectRequestReplyTest`. These are **plain SmallRye Reactive Messaging unit tests — they do not use Quarkus** (JUnit + a Vert.x Proton mock + `MapBasedConfig`, no `@QuarkusTest`/ArC). They also do not rely on a real CBS broker — the `set-token`/`put-token` tests drive a Proton/Vert.x AMQP mock server acting as the CBS node, and only `AzureCbsTokenTest` targets a live Azure Service Bus. No existing test exercises CBS *through Quarkus*, so the QE module would be the first.

`AzureCbsTokenTest` is the richest reference but **`@Disabled`** (never runs in CI) and not self-contained — a hardcoded namespace host and SAS keys read from `TEST_LISTEN_KEY` / `TEST_SEND_KEY` env vars (the token is an HMAC-SHA256 SAS signature built locally, type `servicebus.windows.net:sastoken`). It confirms Azure is **`put-token`-only** (a `set-token` exchange is rejected) and, beyond the happy path, covers refresh, swapped-key / wrong-permission, invalid key, reconnection after a refresh failure, health-probe states and request-reply over CBS — a good checklist to mirror in QE.

There is no CBS coverage in either Quarkus or the Quarkus QE suite. The Quarkus AMQP extension IT (`integration-tests/reactive-messaging-amqp`) starts an **embedded ActiveMQ Artemis** broker (`EmbeddedActiveMQ`) with a custom `ActiveMQSecurityManager` doing plain username/password SASL - Artemis exposes no `$cbs` management node, so this harness cannot exercise CBS as-is. The same applies to AMQP Dev Services, which only supports ActiveMQ Artemis images. (Upstream also skips the embedded-Artemis IT on JDK 25+ because Artemis still requires a `SecurityManager` — [ARTEMIS-4975](https://issues.apache.org/jira/browse/ARTEMIS-4975); the Proton mock avoids this.)

## Planned coverage

1. **`set-token` happy path** — an application with incoming and outgoing CBS-secured channels and a `CbsTokenProvider` bean returning a broker-valid token. Assert a round-trip message and that the connection was authorized via the CBS node (not SASL).

2. **Automatic refresh** — refresh is always active (the connector wraps the base manager in `RefreshingCbsTokenManager`). Two cases: (a) **happy refresh** — a short-lived token with `refreshAt()` well before `expiresAt()`; keep producing and consuming across the refresh boundary (Awaitility) and assert the token is re-fetched, no message loss and no reconnect; (b) **refresh failure** — make the provider's `Uni` fail at refresh time and assert the manager triggers the `onExpiration` path (connection closed / reconnection), surfaced clearly rather than silently stalling.

3. **`put-token` / Azure Service Bus** — the request-reply exchange. Primary target is a live Azure Service Bus namespace with a SAS-token `CbsTokenProvider` (SAS signature built from a shared access key, token type `servicebus.windows.net:sastoken`), gated behind credentials; fallback is a request-reply CBS mock (see open questions). Asserts send/receive over `put-token`, and — mirroring `AzureCbsTokenTest` — that a `set-token` exchange is rejected by Azure, plus wrong-permission (swapped keys), invalid key, refresh and health-probe behaviour.

4. **Failure modes** (JVM-focused) — `cbs.enabled=true` with no `CbsTokenProvider` bean, an unknown `cbs.exchange` value, a provider whose `Uni` fails on *initial* authorization (refresh-time failure is scenario 2b), and an expired/invalid token. Each must surface a clear, actionable error rather than a bare connection drop.

5. **Native smoke test** — scenario 1 in native mode, to confirm the CDI provider bean and token-exchange machinery survive native image build.

## Impact on test suites and testing automation

* A **new module** under the `messaging` folder (e.g. `messaging/amqp-cbs`), analogous to the existing AMQP modules, holding the `set-token`, refresh and failure-mode scenarios against the in-process Proton mock CBS node (see open questions). The mock is exposed as a `QuarkusTestResourceLifecycleManager` wrapping a Vert.x `ProtonServer`, mirroring the existing embedded-`AmqpBroker` resource.
* The mock CBS-node logic (~600 LOC in SR RM) is test-scoped and must be **ported and maintained** in the QE suite - this, not the test code, is the main automation cost.
* The `put-token`/Azure scenario is a **separately gated** test: skipped unless Azure Service Bus credentials are provided, so default CI runs remain self-contained.
* **No OpenShift CBS coverage** via the mock path: an in-test-process Proton server is not reachable from an app pod. OCP coverage would require a containerized CBS node (Qpid) and is deferred (see open questions); the Azure scenario is baremetal-only as well.

## Impact on resources

Expected **low to moderate** impact:

* Estimated execution-time increase of ~2 min (JVM) and a few more minutes for the native build, on baremetal only (no OCP job for CBS), in line with the existing AMQP/`http-minimum`-style modules referenced in [QUARKUS-7048](QUARKUS-7048.md).
* One new module to maintain, plus the ported Proton mock CBS node (no external broker or container needed for the baremetal scenarios).
* An **external dependency** (Azure Service Bus) for the `put-token` scenario — a paid, credential-gated cloud resource that cannot run in the standard pipeline.

## Open questions and future considerations

* **No OpenShift CBS coverage (decided — accept no-OCP).** The chosen in-process
  Proton mock (resolved broker question; see Scope / Impact) is not reachable from an
  app pod, so CBS runs JVM + native on baremetal only. A containerized CBS node
  (e.g. Apache Qpid Broker-J / Dispatch Router) for OCP is out of scope unless
  demand appears.
* **A Quarkus-side extension IT?** There is no CBS coverage in the Quarkus extension
  IT today. Worth confirming with DEV whether one will be added (reusing the SR RM
  mock), so QE coverage complements rather than duplicates it.
* **Azure Service Bus access (decided — gated, off by default).** No Azure account
  today, so the `put-token`/Azure scenario is skipped unless credentials are
  provided. Enable it once there is a support request for the Azure extensions and a
  service account exists. The test must source host and both shared-access keys from
  config/secrets, not hardcode them like the upstream `AzureCbsTokenTest`.
* **RHBQ/IBQ targeting (decided).** SR RM is supported in both RHBQ and IBQ. The CBS
  changes are already in the SR RM version shipped in 3.40 LTS, but as the 3.40 train
  is in motion the coverage targets the next LTS. Target the TS **main** branch only
  for now; backport on demand.
* **Documentation (required).** CBS is documented upstream (SmallRye
  `cbs-authentication` page) but the Quarkus `amqp-reference` guide does not mention
  it. Since `quarkus-messaging-amqp` is fully supported, Quarkus-side docs are a
  must-have — raise a docs issue/PR and track it as part of feature completeness.

## Contacts

* Tester: TBD
