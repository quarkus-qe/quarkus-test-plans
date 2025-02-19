Generate Jackson serializers and deserializers
## Links to the resources
- JIRA ticket: https://issues.redhat.com/browse/QUARKUS-5615
- Serialization:
    - PR: https://github.com/quarkusio/quarkus/pull/41063
    - Follow-up to the original PR: https://github.com/quarkusio/quarkus/pull/46211
    - Documentation PR: https://github.com/quarkusio/quarkus/pull/42289
- Deserialization:
    - PR: https://github.com/quarkusio/quarkus/pull/42954
    - Documentation PR: https://github.com/quarkusio/quarkus/pull/46182
- Related guide: https://quarkus.io/guides/rest#reflection-free-jackson-serialization

## Scope of the testing 
This feature adds a new Quarkus property (`quarkus.rest.jackson.optimization.enable-reflection-free-serializers`) which should not affect behavior of Quarkus.
If this property is activated for a project which uses Jackson for reactive rest endpoints, but doesn't use advanced Jackson annotations (anything, besides @JsonProperty and @JsonIgnore), then Quarkus will use compile-time (de)serialisation, which is a bit faster, than the runtime-one.

## Getting familiar with the feature
- Read the guides:
  * https://quarkus.io/guides/rest#reflection-free-jackson-serialization
  * https://quarkus.io/blog/quarkus-metaprogramming/


## Community status of the extension/feature:
Affected extension (`quarkus-rest-jackson`) is supported, but the feature itself is in Technology Preview.

## Automated test development
To get maximum possible coverage, while also keeping the coverage of classic way, add this property to module, which uses reactive jackson for both serialization and deserialization. Module `rest-client-reactive` looks like a promising candidate. Add test method , which detects, that serialization classes were generated.

Additionally, add tests to Qute module, so it will check that template can be rendered with JSON using the new (de)serialization    

Make sure, that all test run in native mode as well. Update READMEs to reflect changes in both modules. 

#### Impacted resources
The building time may become slightly longer, while current tests should run slightly faster. Both of these changes should be negligible for our use cases. One more test per module may add some time to compilation and running times and consume more RAM, but the impact should be minimal.   

## Contacts
* Tester: Fedor Dudinskii <fdudinsk@redhat.com>