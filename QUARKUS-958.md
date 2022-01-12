# QUARKUS-958 Performance goals for Dragonball

Jira: https://issues.redhat.com/browse/QUARKUS-958

As Spring Boot native is releasing new version we need to do internal performance testing comparing a similar CRUD
application where Quarkus should be faster, smaller and more performant than Spring Boot native.

Quarkus should be number 1 when compared to Spring Boot, Micronaut & Helidon in ALL test cases (JSON, Single Query,
Multiple Queries, etc).

## Scope of the testing

For Dragonball, the application services performance team will provide configurations and analysis of the results from
the performance tests.

The QE goal is to automatically trigger TechEmpower benchmark upon test release promotion.

### Impact on test suites and testing automation
- A job will be created in RHBQ QE Jenkins that will trigger TechEmpower benchmark job in application services
  performance team's Jenkins. This job also pushes results into performance team's result database. This job will be
  ran as part of extended platform support matrix, as we only realistically need the results with candidate releases.

### Impact on resources
- No actual workload will be ran in RHBQ QE Jenkins.

## Advanced topics
- Future consideration on results between product releases in Horreum

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature issue: [QUARKUS-958 Performance goals for Dragonball](https://issues.redhat.com/browse/QUARKUS-958)
* Spring performance subtask: [QUARKUS-1207 Performance Comparison with Spring Boot](https://issues.redhat.com/browse/QUARKUS-1207)
* TechEmpower subtask: [QUARKUS-1206 Tech Empower Performance testing for RHBQ](https://issues.redhat.com/browse/QUARKUS-1206)