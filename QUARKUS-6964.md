# QUARKUS-6964 - Add support for multiple cache types (backends)

JIRA: https://issues.redhat.com/browse/QUARKUS-6964

Add support for multiple cache types (backends)

Upstream PRs:
- https://github.com/quarkusio/quarkus/pull/50007

Documentation:
- https://quarkus.io/guides/cache#multiple-backends-simultaneously

## Scope of testing
This feature is adding option to use multiple cache backend simultaneously.
The tests will verify that it's possible to set 2 cache providers (backends) at same time.
The Caffeine and Redis providers will be used for testing.
The tests coverage will be similar to already created tests for each provider. 
The tests will cover both `@ApplicationScoped` and `@RequestScoped` to check if caching is done correctly.
The testing will cover the ability to cache requests and also invalidate cache.

## Automated test development
For the testing of multiple backend providers at same time the new module with name `multi-provider` will be created.

The automated tests will cover `@ApplicationScoped` and `@RequestScoped` for caching and invalidation requests.
The caching will be tested for default cache key and for the uniquely named cache keys defined by `@CacheKey`.
The invalidation of cache will be also done for default cache key and for uniquely named cache keys.
In addition, the `@CacheInvalidateAll` will be tested.
Testing of invalidation will check if the correct cache entry was invalidated and other are still cached.

For example when having the Caffeine and Redis cache and invalidating the uniquely named Caffeine cache key.
The default Caffeine cache key and all Redis cache should stay intact.

### Impact on TS and resources
The tests will be executed on both baremetal and OpenShift environments, covering JVM and native modes.
Expected time increase on baremetal will be around 1 minute for JVM and 6 minutes for native
For OpenShift the increase will be 3 minutes for JVM and 7 minutes for native.

## References
- https://quarkus.io/guides/cache#multiple-backends-simultaneously

## Contacts
- Tester: Jakub Jedlička <jjedlick@ibm.com>
