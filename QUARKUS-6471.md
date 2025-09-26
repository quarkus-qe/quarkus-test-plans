# QUARKUS-6471 - Support for OAuth2 Protected Resource Metadata (RFC 9728)

JIRA: https://issues.redhat.com/browse/QUARKUS-6471

Upstream PRs: 

- https://github.com/quarkusio/quarkus/pull/48734

This PR adds a partial support for https://datatracker.ietf.org/doc/rfc9728/. It allows Quarkus app to expose URL of authorization server it uses.

## Scope of the testing

Check, that the URL is exposed with `quarkus.oidc.resource-metadata.enabled` property set to true and is not exposed otherwise.
Check, that `quarkus.oidc.resource-metadata.resource` can be used to specify the protected resource. 

## Existing test coverage

There are some tests upstream, but they do not include running against a real OIDC provider.

### Impact on test suites and testing automation

Existing test classes in the `http-advanced` and `http-advanced-reactive` modules will be enhanced with several new test methods.

### Impact on resources

Negligible impact, the test execution time will increase by a couple of seconds.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the feature
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Fedor Dudinsky <fdudinsk@redhat.com>

## References

- https://quarkus.io/blog/secure-mcp-server-oauth2/
- https://quarkus.io/guides/security-oidc-expanded-configuration#resource-metadata-properties