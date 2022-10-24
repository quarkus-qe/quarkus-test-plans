# QUARKUS-2487 Compression support for Reactive Routes and RESTEasy Reactive

Jira: https://issues.redhat.com/browse/QUARKUS-2487

This feature is talking about HTTP body compression on `quarkus-reactive-routes` and `quarkus-resteasy-reactive` extensions

## Scope of the testing

Nothing is compressed by default, but if `quarkus.http.enable-compression=true` then an HTTP response body of a static resource, reactive route and resteasy-reactive resource method is compressed if:

- The method is annotated with `@io.quarkus.vertx.http.Compressed`
- The `content-type` header is set and the value is listed in `quarkus.http.compress-media-types` (`text/html, text/plain, text/xml, text/css, text/javascript, application/javascript` by default).

Reactive route or resource method is never compressed if is annotated with `@io.quarkus.vertx.http.Uncompressed`

If the client does not support HTTP compression then the response body is not compressed.

We are going to cover some gaps and key content-types that we didn't see in upstream as `application/json, application/xml, multipart/form-data`

### Impact on test suites and testing automation

New HTTP endpoints need to be created over `http/jaxrs-reactive` and `http/reactive-routes` in order to cover the scope of this feature. The gzip compression is done by the same library in both extensions, so no need to run native mode tests for both modules. Also, only one of these scenarios (for RESTEasy Reactive) should be run in OpenShift in JVM mode just to verify that there are no issues transferring a compressed content and that no middleware is decompressing the response.   

### Impact on resources:

This is going to increase in about 15 min the total execution time, 5 min in order to run JVM mode in both extensions, 5 min to cover native mode in one extension, and another 5 min to run one scenario on OpenShift. In terms of resources, we don't need to increase any parameter, will re-use the current configuration.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides a clear explanation of expected behavior
- Ensure good user experience and simplicity of use
- Review upstream coverage

## Advanced topics for test development

In environments with extremely low resources, we could end up not only degradating the service throughput but also blocking the main thread. We should throw an exception on those cases so the end user could tunne their service in order to mittigate this `vulnerability`.

## Contacts

* Tester: Pablo Gonzalez Granados <pagonzal@redhat.com>

## References

- [Reactive-routes / http-compression](https://quarkus.io/guides/reactive-routes#http-compression)
- [Resteasy-reactive / http-compression](https://quarkus.io/guides/resteasy-reactive#http-compression)
  