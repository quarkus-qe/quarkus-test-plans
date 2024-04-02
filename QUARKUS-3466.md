# QUARKUS-3466 - Tech Preview - Support role mapping for Client certificates

JIRA: https://issues.redhat.com/browse/QUARKUS-3466

PR: https://github.com/quarkusio/quarkus/pull/37269

Upstream issue: https://github.com/quarkusio/quarkus/issues/23683

Our customers migrating from Fuse 7 on EAP based applications to Camel-Quarkus missed a configuration-based 
mapping for the `SecurityIdentity` roles. This capability was only available programmatically via the `SecurityIdentityAugmentor`.
The customer used mTLS authentication alongside the OIDC authentication and its application used Camel-REST endpoints.
Role-based authorization relied on the path-matching Roles HTTP Security Policy.
Before migrating to the Quarkus, they were using the elytron `x500-attribute-principal-decoder` in the mapper section 
to cut the name out of the complete `CA` name.
Therefore, they required only simplistic roles mapping based on the `CN` attribute defined like this: `cn-name=role1,role2,role3`.

## Scope of the testing
Complement upstream coverage to fully test customer scenario:
* Combine mTLS and OIDC authentication mechanism in one app
* Select the mechanism with path-matching HTTP Security Policies
* Authenticate SSL client on request and enforce authentication with the policy
* Enforce role-based authorization for the mapped roles with a roles policy

Complement upstream coverage to fully test feature:
* Use also PKCS12 as it is slowly becoming a new standard
* Map roles both from the `CN` certificate attribute and from a principal (upstream relies on the `CN` only)
* Remap roles to permissions and use standard security annotation to test permissions (tests HTTP Perms can augment the identity based on mapped roles)

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Existing test coverage
* The Quarkus project contains integration module that tests both in JVM and native mode certificate role mapping when the SSL client auth is required.
* The Quarkus QE Test Suite `security/https` module contains perfect test for this feature. It contains the `SecurityIdentityAugmentor` with comment:
_Quarkus doesn't have a declarative/configuration way to assign roles when using certificate authentication._
The `security/https` module test coverage has no relation to augmentor (different layer, no relation to mTLS), 
hence we can safely replace it with config-based augmentation.
This change will show full advantage of the new feature - simplification.
* The Quarkus QE Test Suite `securit/oidc-client-mutual-tls` module contains mTLS tests between Quarkus application and Keycloak server.
All the required certificates are already there and there is a fair chance that users that use mTLS between Keycloak 
and the app will also want to use it between a front-end and a back-end.
In this module we can simulate our customer scenario with only very small changes that won't affect original test coverage.
This module will test the feature with both JKS and PKCS12.

### Impact on test suites and testing automation
* There will be a couple of extra HTTP requests inside OIDC Client mTLS module. The HTTPS module won't be affected at all. 

### Impact on resources
* No measurable impact, the feature test coverage will be fully integrated into existing coverage as described above.

## Future considerations
* OpenShift test-coverage - OIDC client mTLS test coverage is currently disabled and application setup for mTLS app 
deployed to the OpenShift is going to be challenging
* Currently, we only support principal and CN mapping, there is [opened issue](https://github.com/quarkusio/quarkus/issues/39364)
that will enable mapping for other certificate attributes so that we support the https://datatracker.ietf.org/doc/html/rfc5755#section-4.4.5 specs

## Contacts
* Tester: Michal Vavřík <mvavrik@redhat.com>

## References
- [RFC 5755 - section 4.4.5. Role](https://datatracker.ietf.org/doc/html/rfc5755#section-4.4.5)
- [Explain in the docs how to map the X509 CN attribute to roles](https://github.com/quarkusio/quarkus/pull/39807)