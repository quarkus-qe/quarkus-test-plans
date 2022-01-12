# QUARKUS-1070 Composable Platform streams and non-platform extension discovery

Jira: https://issues.redhat.com/browse/QUARKUS-1070

The goal of this epic is to

* introduce a registry.quarkus.redhat.com for all devtools to consult for looking up platform and extensions
* introduce bom generator in platform that generates uniform but composable BOMs that can be imported individually 
  rather than all in bulk 
* introduce notion of platform stream to indicate which minor version you are tracking and/or what you want to target 
  when using tools

## Scope of the testing
As Red Hat build of Quarkus platform is basically a whole new product distribution channel, the current plan is 
the minimal value proposition to allow us to move ahead with releases. The plan for the platform testing is still
evolving.

A high level overview of the steps needed to automate releases can be found at [Quarkus platform - QE side document](https://docs.google.com/document/d/1qCOu4RiONV2PTTitJzJvRdnXYbUX2au5SXeF3ApupCI).

### Impact on test suites and testing automation
* Red Hat build of Quarkus platform configuration has to be included in Maven repository tests
* Quickstarts with RHBQ supported extensions have to be buildable and runnable with BOMs included in the Red Hat build 
  of Quarkus platform releases
* Code starter tests in start-stop test suite have to pass with code.quarkus portal updated with Red Hat build of 
  Quarkus platform releases
* Mandrel builder container image consideration
  * Initial platform release released with RHBQ core releases needs to be tested with the internal build of Mandrel as 
    Mandrel is usually released along RHBQ core
  * SP releases of the platform need to be tested with the public version of Mandrel as the internal registry can move 
    ahead

### Impact on resources
* The three jobs executed should finish within 2 hours of build promotion
* Red Hat build of Quarkus platform releases are promoted every time a participant releases and update

### Future considerations
For scalability, the release flow has to be automated and schedules of participant product releases need to be 
consolidated.

There are unresolved topics with RHBQ 2.2 release stream:
* What should be included in sources?
* Automated build hand-off from productization to participant QE teams.
* Automated build sign-off by participant QE teams.

## Contacts
* Testers: Michal Jurč <mjurc@redhat.com> ; Rostislav Svoboda <rsvoboda@redhat.com>

## References
* Feature epic: [QUARKUS-1070 Composable Platform streams and non-platform extension discovery](https://issues.redhat.com/browse/QUARKUS-1070)