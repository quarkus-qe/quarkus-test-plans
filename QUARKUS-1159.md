# QUARKUS-1159 - Quarkus FIPS support over RHEL8

JIRA link: https://issues.redhat.com/browse/QUARKUS-1159

The goal is ensure Quarkus runs on a FIPS enabled RHEL 8 System

## Scope of testing

### Impact on test suites and testing automation
- Test will be implemented in https://github.com/quarkus-qe/quarkus-test-suite (OpenShift scenarios are out of scope) 
  
### Impact on resources

FIPS coverage will use Jenkins runner instances with enabled FIPS support and will be executed using JDK 11 and 17 and equivalent Mandrel images. The impact is pretty big because we will run the some test suites twice, in both JVM and native mode. 
The best approach is to execute this coverage with a Jenkins matrix job in order to split the execution between several workers. This approach can reduce the execution time but will require more raw resources in terms of memory and CPU. 

- Jenkins runners for JVM mode: 8 large (8GB RAM + 4CPUs) VM instances
- Jenkins runners for native mode: 8 xlarge (16GB RAM + 8CPUs) VM instances

The above estimation assumes sequential execution using JDK 11 and after that JDK 17. Twice as much resources would be needed on case of parallel execution for both JDK 11 and 17.

## Known issues
### quarkus-smallrye-jwt / quarkus-smallrye-jwt-build:

Currently we are not able to generate a JWT token with FIPS enabled. The main problem is read a private key stored in a "PKCS11 / SunPKCS11-NSS-FIPS" keystore.  

Issue reference: https://issues.redhat.com/browse/QUARKUS-2066

### Keycloak

Currently Keycloak doesn't support FIPS. The official workaround is disable FIPS in keycloak. This patch will not work if you are using a mutual TLS between keycloak and a Quarkus application.  

Issue Ref: https://issues.redhat.com/browse/QUARKUS-2071

### DB2

DB2 is not working over FIPS, a `Connection authorization error` is thrown when a DB2 instance is launched on a FIPS environment. 

Issue Ref: https://github.com/IBM/Db2/issues/43

### Infinispan

Infinispan doesn't support Bouncy Castle libraries as Quarkus security does. [These libraries](https://quarkus.io/guides/security-customization#bouncy-castle-jsse-fips) are the only ones that currently are working over FIPS.  

- [Infinispan](https://github.com/quarkusio/quarkus/issues/25136)

### Native - Bouncy Castle FIPS and BouncyCastle JSSE FIPS - Cannot load new security provider at runtime

Extensions:

    org.bouncycastle:bc-fips
    org.bouncycastle:bctls-fips

Are working on FIPS in conjunction with Quarkus security extension, which are not supported in native mode.

Issue Ref:https://issues.redhat.com/browse/QUARKUS-2064

### Mysql

Reactive datasource is not working with FIPS enabled

Issue Ref: https://issues.redhat.com/browse/QUARKUS-2070

### Strimzi Kafka + SSL / SASL

Strimzi Kafka running over SSL or using SASL authN is not working over FIPS

Issue Ref: https://issues.redhat.com/browse/QUARKUS-1769

### Non-FIPS vs. FIPS coverage details

- Date 2022-07-29
- Quarkus: 1.7.6.CR1

FIPS total amount of tests:     1,830
Non-FIPS total amount of tests: 2,276

Quarkus test suite with FIPS enabled has 20% less coverage than the same test suite with Non-FIPS. 

## Contacts

* Tester: Pablo Gonzalez <pagonzal@redhat.com>

## References
  [General documentation reference](https://www.nist.gov/standardsgov/compliance-faqs-federal-information-processing-standards-fips)
  [RHEL 8 + FIPS](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/assembly_installing-a-rhel-8-system-with-fips-mode-enabled_security-hardening)
  [Certutils](https://fedoraproject.org/wiki/NSS_Tools_:_certutil)