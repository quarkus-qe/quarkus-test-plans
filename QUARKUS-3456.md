# QUARKUS-3456 - Enhance the CLI to be able to encrypt configuration values

JIRA: https://issues.redhat.com/browse/QUARKUS-3456

Upstream issue: https://github.com/quarkusio/quarkus/issues/29172 (partial motivation together with the QUARKUS-3456)

Upstream PR: https://github.com/quarkusio/quarkus/pull/34493

Red Hat Build of Quarkus has ability to handle secrets since the Ghost release.
This feature worked alright, but encrypting secrets so that built-in secret handlers can decode them was somehow awkward.
It was far from trivial to find a correct steps and encrypt the secret in a way Quarkus accepted.
Now Quarkus leverages Quarkus CLI and allows to create, update and remove configuration properties with it.
One of options is to pass the CLI plain secret and Quarkus will encode this secret for you.
You can even ask Quarkus CLI to set encoded secret directly to the application properties.
Or, if you use YAML or other formats (like if you keep secrets in the environment variables), you can just copy it from the CLI output.
We should test this new feature, including other sub-commands under `quarkus config` command as users are likely to use them when they encrypt secrets.

## Scope of the testing
Test coverage must include:

- `quarkus config set/remove/encrypt` command tested with options combined in a fashion customers are likely to use,
- documented flow to create a secret, add it to the Keystore, set Keystore config source and check Quarks application decodes secrets,
- default secret handler `AES/GCM/NoPadding` is used to create the secret,
- secret is decoded in FIPS-enabled environment,
- read encrypted configuration in Quarkus application user code,
- store secrets generated with the CLI in a Keystore and access them using Quarkus application,
- decode the encryption key in Base64, if the plain text key was used to encrypt the secret,
- test help command for all three `config` subcommands to assure [#41131](https://github.com/quarkusio/quarkus/issues/41131) issue is fixed,
- application startup fails when unknown secret handler is used,
- CLI command automatically creates `application.properties` file when it doesn't exist.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Existing test coverage
Upstream test coverage in Quarkus project includes test of encryption with a plain encoded secret, plain secret.
Also, encryption tests with explicitly declared key and with a generated key.
Most importantly, the PR closed [Quarkus issue #29172](https://github.com/quarkusio/quarkus/issues/29172), however it does test it partially and all the capabilities are tested in isolation.

We test secret key handlers in the `config` module of the Quarkus QE Test Suite.

## Impact on test suites and testing automation
Three new integration test classes will be added to Quarkus CLI module of the Quarkus QE Test Suite.
One for each `quarkus config` subcommand (`set`, `remove`, `encrypt`).
Tests will run in a JVM mode only as intention is to test CLI and assure that `application.properties` are updated correctly. 

## Impact on resources
We will start JVM application once per each test class and call an endpoint or two, therefore additional test execution time should be below 30 seconds.

## Future considerations

- use more Ciphers (use different algorithms, modes and padding) to encrypt a secret
- other formats (YAML, env properties)
- support for adding generated secrets into a KeyStore using Quarkus CLI
- [respective Quarkus guide](https://quarkus.io/version/main/guides/config-secrets#protect-the-keystore-password) says you need to store the Keystore password in `application.properties` and suggests to restrict access to the Keystore file itself
  While the principle of least privilege is a good suggestion, we need to verify and document that other config sources can be used to store this secret key so that it is better protected.

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [Quarkus guide - Secrets in configuration](https://quarkus.io/guides/config-secrets)
- [JIRA ticket - QUARKUS-2727 - Encryption of configuration values](https://issues.redhat.com/browse/QUARKUS-2727)
- [Original PR - SmallRye Config ##833 - Secret Keys Handlers](https://github.com/smallrye/smallrye-config/pull/833)
- [SmallRye Config docs - Secret keys](https://smallrye.io/smallrye-config/Main/config/secret-keys/#crypto)
- [Quarkus QE internal tracked](https://issues.redhat.com/browse/QQE-10)
- [Quarkus issue #41131 - missing help text](https://github.com/quarkusio/quarkus/issues/41131)