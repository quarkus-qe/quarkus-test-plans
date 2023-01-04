# QUARKUS-2790 - Switch native GC policy from space/time to adaptive (default)

JIRA: https://issues.redhat.com/browse/QUARKUS-2790

Quarkus documentation: https://quarkus.io/guides/native-reference#garbage-collectors

PR: https://github.com/quarkusio/quarkus/pull/28295

Upstream issue: https://github.com/quarkusio/quarkus/issues/28267

Quarkus Native switched to using the adaptive garbage collector (GC) policy by default in Quarkus 2.13.6.
Previously used space/time policy may demonstrate odd looking memory consumption when the maximum memory allocation pool was not specified and system had more than 3 GB of a physical memory.
The runtime memory consumption of the space/time and the adaptive GC policies does not differ significantly when maximum heap size is specified.
The adaptive policy is supposed to perform better with mid-range workloads, while the space/time policy performs slightly better under heavy stress.
The switch makes sense as the JVM world is moving away from setting the maximum memory allocation and the initial memory allocation pools 
and the adaptive policy should work better when the maximum allocation pool is not set, while maintaining similar memory consumption and performance in the opposite case.

## Scope of testing

Quarkus performance team did an amazing job analyzing and testing different scenarios.
We should verify the impact of the switch on performance and memory in a same fashion as we commonly do. 
More specifically:

- test latency by measuring average time to the first successful request
- indirectly test footprint by measuring average size of memory that the application process has used to load all of its pages (Resident Set Size)
- compare native binary sizes

### Existing tests
[Quarkus start-stop test suite](https://github.com/quarkus-qe/quarkus-startstop) generates, starts, tests, stops small Quarkus applications and measures time and memory.
More specifically, the `StartStopTest#fullMicroProfileNative` test will give us insight on the impact of the garbage collector switch on average microservice application.

## Automated test development
Automated test development is not required as existing test coverage verifies memory management and performance.

## Manual verification
Run `StartStopTest#fullMicroProfileNative` in a thousand iterations and following setting:

- Quarkus 2.13.6.
- Quarkus 2.13.5.
- Quarkus 2.13.6. and the space/time policy

Resulting metrics must be recorder and compared.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Garbage Collector](https://www.graalvm.org/22.1/reference-manual/native-image/BuildOutput/)
- [Provide official API for commonly used svm.jar implementation details](https://github.com/oracle/graal/issues/4862)
- [Memory Management](https://github.com/oracle/graal/blob/master/docs/reference-manual/native-image/MemoryManagement.md)
- [Memory Management at Image Run Time](https://www.graalvm.org/22.0/reference-manual/native-image/MemoryManagement/)