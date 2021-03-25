# QUARKUS-478 Quarkus should provide a first class experience for using native mode on OpenShift Serverless

Jira: https://issues.redhat.com/browse/QUARKUS-478
GitHub: https://github.com/quarkusio/quarkus/pull/11760

The goal of QUARKUS-478 is to provide unified experience of build and deploying Quarkus applications to OpenShift. This
shall be achieved by extending `quarkus-container-image-openshift` extension to enable user to create the build and 
deployment of application with single, unified command for either JVM and native applications with both classic and 
serverless application architecture.

## Scope of the testing

- Define the scope of the testing
- Analyze the possible impact on Quarkus itself - e.g performance, compatibility
- Determine impact on test suites - e.g. new test suite, increased complexity of existing test suite, maintenance burden
- Determine impact on QE infrastructure - e.g. increased complexity of existing automation, maintenance burden, need for new external resources

### Impact on testsuites and testing automation:
- Upstream smoke test [`OpenShiftWithDockerStrategyTest`](https://github.com/quarkusio/quarkus/pull/11760/files#diff-540de2fa8dc0a678c0b39b7fa2700c267fc749dab3ddd6b75a0c5b2f8e9d3171)
  was implemented as part of feature development
- A new deployment strategy utilising docker build strategy will be implemented in 
  [quarkus-qe/quarkus-openshift-test-suite](https://github.com/quarkus-qe/quarkus-openshift-test-suite). Tests in
  `deployment-strategy` module will be extended to also verify this strategy.
- Test covering `quarkus.openshift.jvm-dockerfile=MyDockerFile` and its native equivalent should be implemented.  

### Impact on resources:
The new test implemented in Quarkus OpenShift test suite does not require infrastructural changes and its execution
should be finished within minutes. Test will be executed on all the tested OCP platforms.

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
- Focus on exploratory testing of the features
- Manual negative testing
  - Test with empty Dockerfiles
  - Test with Dockerfiles on invalid path

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature epic: [QUARKUS-478 Quarkus should provide a first class experience for using native mode on OpenShift Serverless](https://issues.redhat.com/browse/QUARKUS-478).
* Upstream feature issue: [OpenShift container image extension](https://github.com/quarkusio/quarkus/pull/11760)
