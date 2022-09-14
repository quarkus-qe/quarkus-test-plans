# QUARKUS-2330 - Promote RESTEasy Qute from TP to full support

This test plan covers Qute functionality provided by `quarkus-qute`,`quarkus-resteasy-qute` and `quarkus-resteasy-reactive-qute` and extensions.

## Scope of the testing
The list below lists the functionality described in the Qute guide [1]. Keyword "covered" means the functionality is covered in upstream tests. We need to create tests for the functionality described as "not covered", "covered partially" or which is important enough to be covered independently of the upstream coverage.
1. Qute.fmt usage
2. Template.render (synchronous)
3. Template.renderAsync
4. Template.createMulti() and createUni()
5. Engine injection
6. Template injection by template name
7. Template injection with @Location tag (template locators)
8. Content:
   1. rendering of tags (parsed and unparsed covered)
   2. expressions (4 types, coverage status unclear)
   3. arrays (covered partially)
   4. loops (covered partially, especially metadata)
   5. character escapes and raw data (covered)
   6. virtual methods (covered)
   7. `if` and `when` sections (covered)
   8. template inheritance (covered, probably worth some addition)
   9. with (covered) and let (covered) sections
   10. eval sections (covered)
9. Encoding: non-latin, RTL and multibyte scripts (this is not covered)
10. Unparsed Character Data (covered upstream but not for multiline)
11. User-defined tags (covered)
12. Values resolvers (covered)
13. Content filters (covered)
14. Direct bean injection (covered) with type-safe Expressions (covered)
15. @CheckedTemplate (covered)
16. Build-name templates: maps (not covered), lists (covered), integers (covered), strings (covered), time (covered)
17. Other templates: @TemplateData (covered), @TemplateEnum (covered), @TemplateGlobal (covered), @CheckedTemplate (covered). These may require special treatment[3] to work in native mode.
18. Content negotiation.
19. Check development mode updates
20. Message bundles (covered upstream, coverage for a single non-trivial case exists in TS). We should add coverage for multiline RTL text.

### Impact on test suites and testing automation:
There are 2 new modules (reactive and non-reactive) planned for QuarkusTS. Tests do not require heavy IO traffic or external connections, so their running time should be comparable to running time of http-minimum module.
- Bare metal
  - 1 minute for JVM mode
  - 4 minute for native mode
- OpenShift
  - 6 minutes for JVM mode
  - 12 minutes for native mode

Additionally, we need to enable qute module in quickstarts (<1 minute running time in JVM mode) for our acceptance and interoperability tests.

## Automated test development
- Create two new test modules under under https://github.com/quarkus-qe/quarkus-test-suite/tree/main/qute.
  - One should cover the reactive module
  - Another should cover the synchronous one.
  - Both should have the same tests but use different outputs in their endpoints.
- Add `qute-quickstart` to the acceptance set and to the interoperability sets.

## Advanced topics for test development
- Getting templates from DB (see [2]), since it is not currently supported in upstream.

## Time estimate
5-10 developer days

## Links

### JIRA links
* https://issues.redhat.com/browse/QUARKUS-2330 (RESTEasy)
* https://issues.redhat.com/browse/QUARKUS-2329 (RESTEasy reactive)
* https://issues.redhat.com/browse/QUARKUS-2328 (templating)

### Manual and reference
- [1] https://quarkus.io/guides/qute-reference
- [2] https://quarkus.io/blog/qute-templates-from-db/
- [3] https://quarkus.io/guides/qute-reference#native_executables 
