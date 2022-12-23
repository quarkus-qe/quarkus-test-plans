# QUARKUS-2744 - Handling a multipart request part as a file based on the content-type

JIRA: https://issues.redhat.com/browse/QUARKUS-2744

Quarkus documentation: https://quarkus.io/guides/resteasy-reactive#multipart

PR: https://github.com/quarkusio/quarkus/pull/29729

Upstream issue: https://github.com/quarkusio/quarkus/issues/29725

Ability to indicate whether a given multipart field should be handled as a file part based on configured content type has been added in Quarkus 2.16 and backported to Quarkus 2.13.6.
This change allows to access the parts as files even though file form parameter name is not known beforehand.

## Scope of testing

Tests should verify:

- file upload is accessible if the file part has a standard content type set with `quarkus.http.body.multipart.file-content-types` configuration property
- file upload is accessible if the file part has a custom content type set with `quarkus.http.body.multipart.file-content-types` configuration property
- any field of form data container class annotated with `@RestForm` with data type `java.io.File` has access to same-named file part

### Existing tests
Upstream test coverage verifies that Quarkus create files on hard drive for each file part with a content type set by `quarkus.http.body.multipart.file-content-types`.
Quarkus QE test coverage sufficiently covers the Multipart Form data handling.

## Automated test development
We should use `org.jboss.resteasy.reactive.multipart.FileUpload#ALL` to access all file uploads as that is most likely how users are going to access file parts with unknown names.
New tests should join existing test coverage in `RESTEasyReactiveMultipartIT` and complete our coverage.

## Impact on test suites and test environment
No measurable impact as tests will be added to existing `RESTEasyReactiveMultipartIT` that runs both in JVM and native mode on bare metal.

### Impact on resources:
No change in needed resources for testing.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [FileUpload interface](https://javadoc.io/doc/io.quarkus.resteasy.reactive/resteasy-reactive-common/2.15.1.Final/org/jboss/resteasy/reactive/multipart/FileUpload.html)