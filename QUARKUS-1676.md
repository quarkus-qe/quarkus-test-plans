# QUARKUS-1676 - Add support for mTLS with OIDC Server

JIRA link: https://issues.redhat.com/browse/QUARKUS-1676

PR: https://github.com/quarkusio/quarkus/pull/23145

Upstream issue: https://github.com/quarkusio/quarkus/issues/19634#issuecomment-999435505

This feature enables an OIDC client to present its TLS certificate when required by an OIDC server 
and thus have both parties of a TLS connection mutually authenticated.

If the server requires client authentication then the client must present its own certificate to the server when
connecting. The certificate is usually taken from keystore, thus mTLS is enabled when an OIDC client can be configured 
to use client certificate from specified file and allows access to the file (by setting keystore pwd, possibly alias and
alias pwd when there is more than one key in a keystore). The OIDC Client now supports multiple formats (PKCS#12, JKS, ...)
for a keystore file, therefore a keystore file type must be newly configurable (same goes for truststore).
OIDC server choice is irrelevant in spite of the RFE as mutual handshake is standardized by [RFC 8846](https://datatracker.ietf.org/doc/html/rfc8446#appendix-E.1).

## Scope of the testing

Tests should verify:
- User is able to add certificates to keystore and truststore to app run in a Java mode and a Native mode
- Certificates and all relevant configurable options are picked up and used correctly by OIDC client
- OIDC client and server communication is secured by mTLS when OIDC server requires so
- Communication with OIDC servers Keycloak and RH SSO (latest and stable version)

### Impact on testsuites and testing automation:

New scenarios are going to be added to Quarkus Test suite with relatively low impact. Each test will
require its own @QuarkusApplication as communication with OIDC server/setting of a keystore/truststore
must be done during a build time.

### Impact on resources:

- No additional requirements for resources in lab
- Overall Quarkus Test suite execution will be prolonged due to the OIDC server container startup time,
  upper runtime should be below 1.5 minute per Keycloak (container) stream and up to 4 minutes for OCP stream.
- Expected total runtime for Keycloak streams is ~ 6 minutes (4 * 1.5 min) as based on testing scope,
  test suite must contain at least 4 Keycloak streams, each stream represent specific:
  - JKS, 
  - PKCS #12, 
  - Invalid keystore,
  - Valid Keystore whose type can't be detected from Keystore file extension
- Expected total runtime for OCP streams is ~ 8 minutes (2 * 4 min) as at least 2 OpenShift deployments
  must be made (one for the latest RH SSO version, one for the stable RH SSO version)
- Keycloak container can take up to 1.6 GiB memory and 350 % CPU, the rest of a test suite resources
  consumption should be below resolution.
- Native image imposes increased memory requirements. Actual consumption is tradeoff between available 
  resources and image run time, therefore prolonged execution time may vary due to available resources.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure documentation uses standard terminology and config options are relatable to Vert.x core options used underneath
- Understand limitations of implementation
- Check keystore types (namely JKS, PKCS12; other proprietary keystore types should be skipped)
- Ensure good user experience and simplicity of use

## Advanced topics for test development

- Dev Services For Keycloak currently (Quarkus 2.8.0. Final) don't support mTLS out of the box (as
  you need to provide Keycloak with a private key and certificate and [DevServicesConfig](https://github.com/quarkusio/quarkus/blob/main/extensions/oidc/deployment/src/main/java/io/quarkus/oidc/deployment/devservices/keycloak/DevServicesConfig.java) provides no such option),
  but user [can provide](https://quarkus.io/guides/all-config#quarkus-oidc_quarkus.keycloak.devservices.image-name) 
  his customized image with mTLS already enabled (see [Setting up TLS section](https://hub.docker.com/r/jboss/keycloak/)).

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [USING OPENID CONNECT (OIDC) TO PROTECT WEB APPLICATIONS USING AUTHORIZATION CODE FLOW](https://quarkus.io/guides/security-openid-connect-web-authentication#mutual-tls)
- [USING OPENID CONNECT (OIDC) AND OAUTH2 CLIENT AND FILTERS TO MANAGE ACCESS TOKENS](https://quarkus.io/guides/security-openid-connect-client#mutual-tls)
- [X.509 user certificate authentication with Red Hat's single sign-on technology](https://developers.redhat.com/blog/2021/02/19/x-509-user-certificate-authentication-with-red-hats-single-sign-on-technology)
- [RH SSO template - sso75-x509-https](https://github.com/jboss-container-images/redhat-sso-7-openshift-image/blob/sso75-dev/docs/templates/sso75-x509-https.adoc)
- [Example of a Keycloak setup](https://gist.github.com/mathieuancelin/0d05905cab009a7d17f99ceddb91c2f0)
- [Private key/certificate options used internally](https://vertx.io/docs/vertx-core/java/#_specifying_keycertificate_for_the_client)
- [Adding a certificate to a RH-SSO truststore, when RH-SSO is deployed on Openshift using reencrypt TLS termination (x509 templates)](https://access.redhat.com/solutions/5708881)
- [Keycloak - Enabling mutual TLS](https://www.keycloak.org/server/enabletls#_enabling_mutual_tls)