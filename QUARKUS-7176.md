# QUARKUS-7176 - Allow to assign a user and roles to a scheduled task

JIRA: https://issues.redhat.com/browse/QUARKUS-7176

Upstream PRs:
- https://github.com/quarkusio/quarkus-security/pull/82
- https://github.com/quarkusio/quarkus/pull/51679

The upstream PRs introduce a new `@RunAsUser` annotation that can be applied to methods annotated with `@Scheduled`.  
The annotation allows configuring a `SecurityIdentity` that is active only for the duration of the scheduled task execution, 
enabling scheduled jobs to interact with security-protected application code.

## Existing test coverage
Upstream test coverage include validating the core functionality of the `@RunAsUser` annotation.

Existing coverage includes:
- creation of a `SecurityIdentity` based on `@RunAsUser` attributes
- interaction with security annotations such as `@Authenticated` and `@RolesAllowed`
- behavior across synchronous and asynchronous scheduled method return types
- access to identity information via `SecurityIdentity` and `CurrentIdentityAssociation`
- build-time validation of supported `@RunAsUser` usage

## Scope of the testing
Testing will focus on runtime integration behavior of scheduled tasks executed with and without an assigned user identity.

### Default scheduled execution (no assigned identity)
- Verify that calling an `@Authenticated` method from a `@Scheduled` task without `@RunAsUser` is not allowed

### Scheduled execution with assigned identity
- Verify that a `@Scheduled` task annotated with `@RunAsUser` is treated as authenticated when calling a method marked `@Authenticated`

### Role-based authorization for scheduled tasks
- Verify that a `@Scheduled` task annotated with `@RunAsUser` can call a `@RolesAllowed` method when the required role is assigned
- Verify that access to `@RolesAllowed` logic is denied when:
  - no identity is assigned to the scheduled task
  - the required role is not included in `@RunAsUser`

### Unrestricted access
- Verify that a `@Scheduled` task without `@RunAsUser` can invoke restricted application logic that is explicitly relaxed using `@PermitAll`

### Failure isolation between scheduled tasks
- Verify through concurrent execution and independent observable results that failures caused by authorization checks in one `@Scheduled` task do not prevent other scheduled tasks from executing successfully

## Impact on test suites and testing automation
The `scheduling/quartz` module in the Test Suite will be extended with new tests covering the scenarios listed above.

## Impact on resources
The expected execution time will be increased about 1,5 minutes for JVM mode and 5 minutes for native mode.
On OCP, the execution time is expected to increase by around 5 minutes for JVM mode and 8 minutes for native mode.

## Getting familiar with the feature
- Review the `@RunAsUser` annotation
- Understand how `@RunAsUser` integrates with the scheduler execution lifecycle
- Review documentation section:
  - https://quarkus.io/version/main/guides/scheduler-reference#assign-a-user-and-roles-to-a-scheduled-task
- Review upstream unit test coverage to understand:
  - interaction with security annotations (`@Authenticated`, `@RolesAllowed`)
  - expected identity scoping semantics
  - validation rules and supported usage

## Contacts
* Tester: Georgii Troitskii <gtroitsk@ibm.com>

## References
- https://quarkus.io/version/main/guides/scheduler-reference#assign-a-user-and-roles-to-a-scheduled-task
