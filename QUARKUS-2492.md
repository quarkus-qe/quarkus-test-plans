# QUARKUS-2492 - Transaction API

JIRA link: https://issues.redhat.com/browse/QUARKUS-2492

Programmatic distributed JDBC transaction API based on Narayana library

Documentation:
[Narayana Documentation](https://www.narayana.io/)
[Quarkus JTA](https://quarkus.io/guides/transaction#programmatic-approach)

## Scope of the testing
Test development will focus on
- Quarkus JDBC transactions based on programmatic approach
- Quarkus JDBC transactions based on lambda approach
- Verify commons transactions states as "begin", "commit" and "rollback"
- Verify classic and reactive flavors
- Verify several supported semantics such as "DISALLOW_EXISTING", "JOIN_EXISTING", "REQUIRE_NEW", "SUSPEND_EXISTING"

### Impact on test suites and testing automation
We are going to create two new modules (classic and reactive) under SQL/DB folder, in order to develop new JDBC transaction scenarios. 
The main idea is to develop a "real scenario" where the transactions are handler in a service layer, rather than in entity level. 
This new module is going to increase the compile time, especially in native mode, and is important to run the scenario in 
all the platforms, OCP, Baremetal and native mode. I would say that each native compilation could takes about 3 min and then 
1 extra minute in order to run the scenario itself. Overall my prediction is that the total amount of time required to run 
transaction API scenarios is about 15 min.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation is clear and easy to understand
- Ensure good user experience and simplicity of use
- Review upstream coverage

## Advanced topics for test development

Narayana JTA transaction support several thread semactics such as "DISALLOW_EXISTING", "JOIN_EXISTING", "REQUIRE_NEW", "SUSPEND_EXISTING". Currently we are just covering the default one (requireNew), but would be great in the future to cover the others. 

## Contacts

* Tester: Pablo Gonzalez Granados <pagonzal@redhat.com>