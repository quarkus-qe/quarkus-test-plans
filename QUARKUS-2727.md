# QUARKUS-2736 - Encryption of configuration values

JIRA: https://issues.redhat.com/browse/QUARKUS-2727

PR: https://github.com/quarkusio/quarkus/pull/31824 (https://github.com/smallrye/smallrye-config/pull/833 on SmallRye Config end)

Upstream issue: https://github.com/quarkusio/quarkus/issues/31621

Upstream documentation: https://quarkus.io/version/main/guides/config-reference#secret-keys-expressions

SmallRye Config [has been enhanced](https://github.com/smallrye/smallrye-config/pull/833) with ability to decrypt or decode secret key values.
Secret key value is resolved from Secret Keys Expression provided by any config source. The expression consist of a Secret Keys Handler name and actual Secret Key.
Built in interceptor recognize this expression, find the handler by the name and handler transforms Secret Key to actual value.
There are also handlers provided by the SmallRye Config out of the box (Jasypt, Crypto) that supports basic encryption algorithms.

It is important to verify ability to load secret values from KeyStore as Red Hat Build of Keycloak, Red Hat Process Automation Manager and other products may need this ability.
In cloud environment, our customers are most likely to store secrets in OpenShift Secrets, and we (Red Hat Build of Quarkus QE team) are best posed to test 
full application flow, starting in OpenShift Secret and down to the configuration property injected to resource.

## Scope of testing

Tests should verify:

- creating of a custom `SecretKeysHandler` and provide different ways to decode or decrypt configuration
  values
- `SecretKeysHandler` registration via the `ServiceLoader` mechanism (both directly and through factory)
- `SecretKeysHandler` registration to manually built config as we do for Secret Keys Names
- any config source can contain build time and runtime secret keys expressions:
  - `application.properties`
  - password secured KeyStore loaded by KeyStore Config Source (`smallrye-config-source-keystore` dependency) using:
    - custom handler
    - out of the box handler
  - OpenShift secret accessed with Quarkus Kubernetes Config extension as:
    - file system config secret (`quarkus.openshift.app-secret`)
    - API server config secret (`quarkus.kubernetes-config.secrets`)
  - custom config source
- config profile dependent secret keys expressions
- using multiple secret keys handlers:
  - custom secret keys handlers
  - Jasypt secret keys handler
  - out-of-the-box handlers provided by Crypto (`smallrye-config-crypto` dependency)
- modification of secret keys expressions and secret keys handlers in DEV mode
- secret keys handlers in native mode and make sure all the handlers (custom and built in) are registered for reflection automatically
- secrets hidden from accidental exposure by `SecretKeysConfigSourceInterceptor`, that is secrets that are also Secret Keys
- encryption key used to decode secrets encoded by the `AES/GCM/NoPadding` algorithm. The point here is to verify algorithm emphasized in upstream documentation.
- basic encryption capabilities provided by Jasypt handler

### Existing tests

The Quarkus Test Suite contains Secret Keys Names in its config module.
Customers are likely to use them together with secrets and so should we.
Upstream coverage is mostly secured by SmallRye Config project and test coverage inside Quarkus is sporadic.
That means we need fully verify functionality in DEV mode and native mode.

## Impact on test suites and test environment

New scenarios are going to be added to the `config` module of the Quarkus Test Suite.
Integrating scenarios into existing tests `ConfigIT`, `OpenShiftFileSystemConfigSecretConfigIT` and `OpenShiftApiServerConfigSecretConfigIT`
will not only be considerate to the resources (we don't need to build and start new application), but also makes sense content wise.
We already test there [Secret Keys Names](https://smallrye.io/smallrye-config/Main/config/secret-keys/#secret-keys-names)
and OpenShift secrets respectively.

New DEV mode test will be required to make sure changes in watched files are reflected and secrets updated.
We should also update secret keys expressions (exchange handlers and so on).

### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be around 1 minute for tests added to existing classes (please see above).
  That includes HTTP requests, secrets loading, encryption and decryption and result evaluation. 
  The DEV mode test will add extra 1 minute to test execution time.
- No measurable additional requirements for a Native image scenario

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Advanced topics for test development

- Apply additional converters, config sources and interceptors on secrets

## Future considerations

- SmallRye Config [plans following improvements](https://github.com/smallrye/smallrye-config/pull/833):
  - ability to configure SecretKeysHandler
  - certificate support
  - provide out of the box handlers
- Quarkus CLI might have a plugin, that will help with encrypting values

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [Secret Keys](https://smallrye.io/smallrye-config/Main/config/secret-keys/)
- [Update Quarkus version in PIM to support Keystore and trustore passwords to be stored on vault](https://issues.redhat.com/browse/RHPAM-4423)