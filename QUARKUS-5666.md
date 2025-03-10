# QUARKUS-5666 Add support for encrypted PKCS#8 - technology preview

### Links to the resources

- JIRA ticket: https://issues.redhat.com/browse/QUARKUS-5666
- GH PR related to the JIRA issue: https://github.com/quarkusio/quarkus/pull/44549
- Guide modified and related:
    * https://quarkus.io/guides/tls-registry-reference

## Scope of the testing
The goal of this testing is to ensure that this new feature introduced into Quarkus TLS Registry can successfully handle encrypted PKCS#8 (PEM) Keys.
The tests validate both server and client side functionalities when using encrypted PEM certificates.

**Note** : The tests won't cover encrypted PEM in a FIPS-enabled environment. This limitation is due to the lack of support for required encryption/decryption algorithms
(specifically AES-128-CBC) by FIPS-compliant providers.
Attempting to use encrypted PEM certificates under FIPS results in a NullPointerException error due to null algorithm parameters.
This is a known limitation at the Quarkus and SmallRye level. An enhancement issue has been created in Quarkus
https://github.com/quarkusio/quarkus/issues/46696 to track the improvement of the clarity's error. 

### Changes in the Test Framework

To facilitate the testing, the Test Framework has been modified with the following changes:
   * Adding smallrye-private-key-pem-parser dependency to decrypt the new encryptition parser added EncryptedPKCS8Parser()
   * Introduce `io.quarkus.test.services.Certificate.Format.ENCRYPTED_PEM` in the TF’s @Certificate annotation to distinguish between PEM and encrypted PEM files.  
   * Support encrypted PEM key generation and decryption
   * Verifying Quarkus to handle encrypted PEM (PKCS#8) files
   * Ensuring that Quarkus can properly decrypt the private key during startup when a password is provided.

#### What to test

- The TLS registry is able to load encrypted PEM and decrypt it. 
- Mutual TLS using encrypted PEM, verify Quarkus REST client can use encrypted client certificates to communicate with Quarkus REST endpoint that also uses encrypted certificates. 
- gRPC communication with encrypted Pem, verify you can use gRPC client to communicate with Quarkus gRPC server (running on the same server, not the separate one) that uses encrypted PEM file.
- Https communication using encrypted Pem,  ensure you can communicate with Quarkus REST endpoint using HTTPS (no client-side authentication).
- Certificate reloading, validate with newly generated certificate, it works for encrypted PEMs as well .
- Injecting TLS registry configuration and can see the private key decrypted (so you can see keystore and check some x509 attributes).

## Getting familiar with the feature

Checking and looking through the PRs related and guides:

* https://quarkus.io/guides/tls-registry-reference
* https://github.com/quarkusio/quarkus/pull/44549


## Existing test coverage
There is no existing test coverage about it in our test suite, and either in the test framework but 
we will add smoke testing coverage in the quarkus-test-framework to include in examples/https

#### Impacted on resources
The Quarkus test framework would be impacted with slight increase of time execution of the examples/tests in TF

## Contacts

* Tester: Jose Carranza <jcarranz@redhat.com>