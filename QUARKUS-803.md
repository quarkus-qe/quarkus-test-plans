# QUARKUS-803 - Test and verify Red Hat DataGrid as part of the the RHBQ QE process

JIRA link: https://issues.redhat.com/browse/QUARKUS-803

Red Hat Data Grid is in-memory, distributed, NoSQL solution. It provides faster access to the data than traditional relational database solution, through caching, near caching
and operates in-memory rather than in computer storage. Users can benefit from this by storing in cache commonly accessed data that does not change frequently.
Worth weighing rationality of using in-memory data grid as it introduces additional complexity to the system. The rule of thumb can be the number of frequent request made 
to the service, if this number is high and queries to the database are complex (multiple joins and conditions) then it can speed up the system by caching these results.


## Scope of the testing
 - Update OpenShift clusters to use RHDG Operator 8.2
 - Setup RHDG clusters and specify number of nodes
 - Make use of Serialization to store user defined types of objects (incorporates Protobuf and Marshaller)
 - Make use of Querying (works with Serialization only)
 - Incorporate relational database into test infrastructure
 - Setup Data Eviction (by memory/entries threshold and entry's TTL)
 - Migrate Authentication and Encryption solution from [infinispan-client](https://github.com/quarkus-qe/quarkus-openshift-test-suite/tree/main/infinispan-client) scenario
 - Migrate Failover scenario from [infinispan-client](https://github.com/quarkus-qe/quarkus-openshift-test-suite/tree/main/infinispan-client)
 - Optional: Near Caching

 Given: Client is authenticated and encryption is used for every request  
 Data from relational database are loaded into RHDG cache. Any simillar subsequent GET request returns data from RHDG server (if near cache is configured, then
 directly from RHDG client). After update of the entry, the entry must be stored in relational database and RHDG cache must be invalidated for that entry.

### Impact on resources:
- 1 Red Hat Data Grid Operator
- N Red Hat Data Grid server nodes

### Impact on testsuites and testing automation:
- Test will be implemented in https://github.com/quarkus-qe/quarkus-test-suite using [quarkus-test-framework](https://github.com/quarkus-qe/quarkus-test-framework)  

## Getting familiar with the feature
Following actions were taken to ensure familiarity:
 - Focus on exploratory testing of the feature
 - Ensure good user experience and simplicity of use

## Advanced topics for test development
 - Performance testing with and without RHDG upon specified test data
 - Test of data replication amongs RHDG cluster (adding/removing RHDG node)

These advanced topics are listed to be considered, there is no guarantee to have sufficient manpower and time to focus on them.