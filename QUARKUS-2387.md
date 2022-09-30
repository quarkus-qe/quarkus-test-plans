# QUARKUS-2387 - Promote Mailer extension from TP to full support

Evaluate the extension for promotion to full support. Propose needed enhancements and report found bugs.
Mailer extension is focused on sending emails over SMTP, the extension wraps vertx-mail-client and enables its usage in native mode.
The extension is not targeting functionality and end-user experience available in email marketing and templating services like Sendinblue or Bee.

## Scope of the testing
 - Review of test coverage in Quarkus integration tests
 - Review Quarkus Quickstart for the extension
 - Review upstream guides for the extension
 - Exploratory testing of the extension
   - Using Gmail configuration, optionally SendGrid
   - Customizations like non-Gmail `quarkus.mailer.from`
   - File attachments with custom file names
   - HTML emails, headers for message body
   - Non-ascii characters in email address, subject, body
   - Friendly part (e.g. name) before email address
   - Compilation into native, execution of the binary
 - Implement test coverage for missing areas

### Impact on test suites and testing automation
 - No impact on test automation is expected
 - Mailer extension is matured one, no additional test coverage is expected to be needed
   - Exploratory testing of the extension may reveal one or two areas to cover

### Current test coverage
#### vertx-mail-client repo
```bash
git grep "@Test" | wc -l
     379
```
#### quarkus repo
```bash
grep "@Test" extensions/mailer -R | wc -l
      48

grep "@Test" integration-tests/mailer -R | wc -l
       8
```

## Automated test development
 - Depends on exploratory testing results

## Links
* https://issues.redhat.com/browse/QUARKUS-2387
* https://quarkus.io/guides/mailer
* https://quarkus.io/guides/mailer-reference
