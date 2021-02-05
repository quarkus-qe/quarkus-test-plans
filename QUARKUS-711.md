# QUARKUS-711 - RESTEasy support for Multipart data

JIRA link: https://issues.redhat.com/browse/QUARKUS-711

Test plan focuses on RESTEasy support for multipart data provided by `quarkus-resteasy` and `quarkus-resteasy-multipart` extensions, as described in:
- https://quarkus.io/guides/rest-json#multipart-support
- https://quarkus.io/guides/rest-client-multipart

Apart from topics described below, the functionality remains unchanged.

## Scope of the testing
- Testing verifies:
    - Correct multipart request data processing.
    - The new default value of charset of parts of a multipart request is `UTF-8` (in contrast to previous `us-ascii`) if the encoding is not specified
    - The value of the charset is overridden by property `quarkus.resteasy.multipart.input-part.default-charset`.
    - The value of content type is specified by property `quarkus.resteasy.multipart.input-part.default-content-type`.
- `quarkus-resteasy-multipart` extension replaces `org.jboss.resteasy:resteasy-multipart-provider` dependency in affected applications.

## Automated test development
- Review relevant modules in `quarkus-quickstarts` project.
    - Update `rest-client-multipart-quickstart` module to use `quarkus-resteasy-multipart` extension instead of the `org.jboss.resteasy:resteasy-multipart-provider` dependency.
- Add a test case with a multipart scenario into `beefy-scenarios` (`001-quarkus-getting-started` module).
    - Test request/response with multipart body containing set of small images, binary data, text with diacritics.
