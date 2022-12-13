# QUARKUS-2481 - Allow to provide custom configuration for JAXB context

JIRA link: https://issues.redhat.com/browse/QUARKUS-2481

Allows to the end user to provide  a custom JAXBContext configuration

Documentation: https://quarkus.io/guides/resteasy-reactive#advanced-jaxb-specific-features
Related to: [QUARKUS-2019](QUARKUS-2019.md)

## Scope of the testing
- Review upstream coverage
- Review corner cases and possible security vulnerabilities
- Test development in case of missing test coverage 

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature

## Automated test development
 - No need for additional test coverage, PR with the feature is well done.
 - https://github.com/quarkusio/quarkus/pull/25801/files contains reasonable feature coverage

## Advanced topics for test development
N/A

## Contacts

* Tester: Pablo Gonzalez Granados <pagonzal@redhat.com>