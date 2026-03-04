# QUARKUS-7169 Introduce a quarkus Maven packaging and an assorted lifecycle

JIRA: https://issues.redhat.com/browse/QUARKUS-7169

Upstream PRs:
* https://github.com/quarkusio/quarkus/pull/51636
* https://github.com/quarkusio/quarkus/pull/51587

Quarkus 3.31 introduces a new maven packaging `quarkus` which should provide smaller and faster builds.
This packaging should be default for new apps, created by quarkus CLI.
Next to other optimizations, it should mainly skip building the default maven jar `target/app-version.jar`.
This jar is almost never used, since quarkus is run via `target/quarkus-app/quarkus-run.jar` anyway.
With `quarkus` packaging the pom.xml will also simplify, as `quarkus-maven-plugin` will no longer have to list execution goals, to connect to.

## Scope of testing
New packaging should provide little testable changes.
With the new packaging itself we will only test, that default maven jar is not created and app still works properly.
This change should make the quarkus build faster, but verifying that is out of scope of this testing.

Another change is to quarkus CLI.
With this change, CLI should create apps with `quarkus` as default packaging. 

Application generated with `code.quarkus` should also have `quarkus` as default packaging.  

## Existing test coverage
Quarkus QE TS has no coverage for `quarkus` packaging.

## Impact on test suite
A new test module `packaging/quarkus` will be created, containing tests for quarkus packaging.
Another check will be added into `quarkus-cli` and  module, to check that new apps has the quarkus packaging as default.

Additional check will be added into startstop TS, checking that `code.quarkus` also generates apps with default `quarkus` packaging.

## Impact on resources
New module will be created. It will only be run with JVM on bare metal and OCP.
Expecting additional execution time of few minutes.

## Contacts
* Tester: Martin Ocenas <mocenas@ibm.com>

## Additional resources
* [1] - https://quarkus.io/blog/building-large-applications/#doing-less
* [2] - https://github.com/quarkusio/quarkus/wiki/Migration-Guide-3.31#new-quarkus-packaging-and-maven-lifecycle