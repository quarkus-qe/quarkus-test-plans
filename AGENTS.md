# Agent Instructions for Creating Quarkus Test Plans

Use this file as operating guidance when drafting or revising a Quarkus feature test plan in this repository.

Think like a senior engineer evaluating a feature for production use.

Produce a test plan that is:
- easy to review
- easy to execute
- explicit about scope, ownership, risks, and resource impact
- aligned with repository conventions and strong existing examples

## Repository Overview

This repository contains test plans for Quarkus product features and enhancements. It is a documentation-focused repository
that tracks quality engineering efforts, test coverage strategies, and testing automation plans for the Quarkus framework.

## Mandatory repository conventions

- Name the file with the issue key, for example `QUARKUS-123.md`
- Use PR and commit titles in the form `QUARKUS-ABC - TITLE_OF_THE_RFE`

## Required structure

Test plans must include the following content. Section names and order can vary based on what makes the most sense for
the specific feature, but all essential information must be present:

1. Title with issue key and feature name
2. JIRA link
3. Quarkus documentation and upstream links
4. Short feature summary
5. Scope of testing, extension interactions
6. Getting familiar with the feature
7. Existing coverage
8. Planned coverage
9. Impact on test suites and automation
10. Impact on resources
11. Open questions or future considerations
12. Contacts
13. References

## Authoring rules

### 1. Start with a short feature summary
State:
- what the feature is
- why it matters
- important support limits or constraints

Do not start with vague background text.

### 2. Add all important links
Include when available:
- JIRA
- Quarkus documentation
- upstream issues
- upstream PRs
- follow-up PRs
- related test plans
- community status

Do not force reviewers to search for core references.

### 3. Define scope precisely
State:
- what is in scope
- what is out of scope
- target environments
- target modes such as JVM, native, dev mode, OpenShift

Do not leave scope implied. Native mode justification can be skipped because Quarkus fully supports both JVM and Native mode.

### 4. Assess extension interactions and integration points explicitly
Consider and document how the feature interacts with other Quarkus extensions and cross-cutting concerns.
Think about:
- Which extensions users might combine with this feature
- Common integration patterns (observability, security, data access, etc.)
- Potential conflicts or special considerations

### 5. Be specific and justify coverage decisions
Name exact test targets (modules, platforms, architectures) and explain why each was chosen.
Examples:
- "Testing module X because it represents typical usage"
- "Excluding native mode due to upstream validation"
- "Expanding compatibility matrix to cover production environments"

Do not use generic statements or list scenarios without rationale.

### 6. Review documentation as part of testing
Check:
- correctness
- completeness
- usability
- whether documented configuration is covered by tests

Documentation review is part of feature validation.

### 7. Record blockers and open questions
Capture:
- missing services or operators
- unsupported environments
- unresolved release questions
- testing blockers versus functional blockers

This section is optional in the test plan.

### 8. Estimate resource impact
State expected impact on:
- execution time
- cluster or machine usage
- pipeline complexity
- external dependencies
- maintenance burden

Rough estimates are acceptable if clearly explained.

## Review guidelines

When reviewing test plans:
- Inspect referenced implementation and documentation
- Review test plan with authoring rules in mind
- Provide a concise summary (4-5 bullet points) of areas that are covered well in the test plan
- Provide a concise summary (4-5 bullet points) of areas for improvement, and add a priority prefix to each bullet point
- Provide a rating (0-10) of the test plan, 10 means excellent
- Don't write any report document, present the summary directly

## Key principle

**Content over structure**: A test plan with flexible structure but complete, well-justified content is better than one
that follows a rigid template but lacks depth or clarity.
