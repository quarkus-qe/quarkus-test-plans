# QUARKUS-6292 - Extend the platform with offering

JIRA: https://issues.redhat.com/browse/QUARKUS-6292

Extend the platform with offering

## Scope of testing
QUARKUS-6292 start dividing the offering by adding the offering metadata each supported extension.
The metadata label should have format of `<offering-id>-support`, where the extension can have 0-n labels.

As adding the multiple offering labels, there are 3 category which should be adjusted/checked:
1. Quarkus registry (registry.quarkus)
2. Code Quarkus (code.quarkus)
3. Quarkus CLI

### Quarkus registry (registry.quarkus)

For registry there is no need for any update as the offering labels are part of already existing metadata.
Currently only offering label in metadata is `redhat-support`.

### Code Quarkus (code.quarkus)

Update of Code Quarkus is defined in https://issues.redhat.com/browse/QUARKUS-6293.
You can find the scope of testing and planned test development at https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6293.md

### Quarkus CLI

Update of Code Quarkus is defined in https://issues.redhat.com/browse/QUARKUS-6397.
You can find the scope of testing and planned test development at https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6397.md


## Automated test development

You can find the description for the test development in these test plans:
* Code Quarkus https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6293.md
* Quarkus CLI https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6397.md


## References
- https://issues.redhat.com/browse/QUARKUS-6292
- https://issues.redhat.com/browse/QUARKUS-6293
- https://issues.redhat.com/browse/QUARKUS-6397
- https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6293.md
- https://github.com/quarkus-qe/quarkus-test-plans/blob/main/QUARKUS-6397.md

## Contacts
- Tester: Jakub Jedlička <jjedlick@redhat.com>
