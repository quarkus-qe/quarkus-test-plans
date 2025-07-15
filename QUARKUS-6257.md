# QUARKUS-6257 Quarkus AI testing strategy

JIRA links: https://issues.redhat.com/browse/QUARKUS-6257, https://issues.redhat.com/browse/QQE-1584

# Scope:
We (Quarkus QE team) should verify that Quarkus AI is in good shape to be released to customers. For the purpose of this document, Quarkus AI means a set of extensions from the Quarkus LangChain4j group (currently part of the Quarkiverse), which will be productised.

## Requirements for passing
For every release, we should verify that:
1. All extensions targeted for productisation are clearly defined (i.e. there is a list of them).
2. There are defined supported levels for these extensions, including their features, the used LLMs, and platforms.
3. Extensions are properly productised and included in the released Quarkus platform.
4. The required features of these extensions operate on selected platforms, utilizing specific LLMs, without regressions, and with reasonable resource consumption.
5. The features are documented, there are guides in upstream and these features (and their support levels) are described in product documentation.
6. The extensions are properly handled by Quarkus tooling (see below).
7. There is no bugs of Critical level or above for the Quarkus AI.

## Additional requirements for full support
This list is based on Red Hat scope of coverage: https://access.redhat.com/support/offerings/production/soc/
1. There are starter code examples for key extensions.
2. There are detailed (upstream) guides and property references.
3. There is integration with existing monitoring tools for Quarkus (observability, metrics and logging).
4. CVEs in these extensions and their dependencies are detected by productisation tooling.
5. Unless it is a major release, all changes should be backwards compatible.
6. The functionality works on both main and additional target platforms.
7. DEV team is confident in providing full support to the extensions.


# Out of scope
There are things, that we do not test:
1. Unit tests of extensions, or integration tests, which use mocking. We make sure, that these tests exist upstream and run regularly, but we do not run them ourselves, nor put much effort into writing or supporting them.
2. Performance of AI models (or services) themselves. We do not check capabilities of the models, their resource consumption or the validity of the answers.


# Testing methodology
## Tests of productisation
To ensure that the extensions are properly productised, we should utilize our existing productisation test suites (e.g., MaReTe) and adapt them to changes in the platform.
We should ensure, that Quarkus tooling works properly with productised bits: they are accessible in code.quarkus instances, cli can add these extensions, they work in dev mode.

## Unit tests
To verify that the extensions in upstream can be compiled and built, we should make sure that unit tests are implemented in upstream and run regularly.

## Feature tests
We should automate the tests for product bits as much as possible. We should also prefer to use upstream tests for testing of product or move our tests upstream. We should ensure that upstream tests are run in CI regularly.

## Documentation verification
Should be done manually.

# Test environment and tooling
The tests should be run on baremetal and OpenShift, on both x86_64 and ARM64 machines. Main target OS is RHEL 8.
Initially, we may be using only some of them.

We will be using Jenkins for running tests, internal OpenShift clusters for testing apps on OpenShift, and OpenStack for testing apps on baremetal, as well as AWS for ARM testing.
We may also need to deploy LLMs running on separate nodes on OpenShift, OpenStack, or AWS. We may use cloud services that provide them (OpenAI or Watsonx), which depends on the available budget for the services.

# Release control
Quarkus AI is a part of ordinary Quarkus releases and conforms to the same timetable and build process.

# Risks
1. Quarkus AI is not passing the requirements (see above).
Actions: notify Product Manager, agree on the mitigation activities (e.g. limiting the scope), or move support of this feature to a later release.
2. We do not have enough resources or time to do the proper verification.
Actions:
This risk should be constantly monitored. If it arises, ask the PM  as early as possible to limit the scope or provide us with more resources.
Things to pay attention to:
   1. Do we have coverage for all existing features and platforms fast enough?
   2. Are there any blockers on the development/productisation side that will stop us from starting verification in time?
   3. Do we have access to all tools (e.g., credits for cloud-based AI tools or hardware, powerful enough to run the tests) that we need for coverage?

# Review and approval
This strategy should be reviewed and approved by members of Quarkus QE team (including both senior ones).
