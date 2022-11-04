# QUARKUS-2483 - Kubernetes service binding support for Reactive SQL clients

Jira: https://issues.redhat.com/browse/QUARKUS-2483

PR: https://github.com/quarkusio/quarkus/pull/25657

In Quarkus 2.10 was added Kubernetes service binding support for 5 Reactive SQL Clients extensions.

Clients with the [Production support](https://access.redhat.com/support/offerings/production/soc/):
- Reactive PostgreSQL client
- Reactive MySQL client

Clients with the [Technology Preview features support](https://access.redhat.com/support/offerings/techpreview):
- Reactive MS SQL client

Clients with Quarkus community support:
- Reactive DB2 client
- Reactive Oracle client

At the moment of writing, Quarkus [lists only 3 operators](https://quarkus.io/guides/deploying-to-kubernetes#automatic-service-binding) 
that support the service auto-binding:
- [CrunchyData Postgres](https://operatorhub.io/operator/postgresql) for PostgreSQL
- [Percona XtraDB Cluster](https://operatorhub.io/operator/percona-xtradb-cluster-operator) for MySQL
- [Percona Mongo](https://operatorhub.io/operator/percona-server-mongodb-operator) for MongoDB, but there is no reactive MongoDB client

MySQL operator is a community operator and has not been vetted or verified by Red Hat and its stability is unknown.
Service binding for the Reactive SQL Clients is provided by one service factory common for all the extensions, so support for
each client is mostly done by providing `ServiceBindingConverter` - additional code in each extension is minimal.

Current test coverage for Service Binding in the Quarkus Test Suite consist of CrunchyData Postgres test for `quarkus-jdbc-postgresql` extension.
Considering Quarkus Kubernetes Service Binding is still a preview, absence of suitable operators and extensions level of support, we should only add analogous test for `quarkus-reactive-pg-client` extension.

## Scope of the testing

Tests should verify automatic datasource binding:
- Quarkus application can connect and work with PostgreSQL database without configuring credentials, connection string etc.

### Impact on testsuites and testing automation:

New module called `service-binding/postgresql-crunchy-reactive` is going to be created in the Quarkus Test suite to verify
that Quarkus `kubernetes-service-binding` is able to inject application projection service binding from a PostgreSQL cluster
created by Crunchy Postgres operator. Only OpenShift tests are required, one for JVM mode and one for native mode.
Init SQL script will insert an entity to database, test will call resource endpoint and retrieve the entity from database.
Should binding fail, the application won't be available or won't be able to retrieve the data.

### Impact on resources:

- No additional requirements for resources in lab
- Required additional time for the test execution should be below 10 minutes
- No measurable additional requirements for a Native image scenario

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Ensure documentation provides clear explanation on configuration options
- Ensure good user experience and simplicity of use

## Advanced topics for test development

None.

## Future considerations

- Service Binding without database operators for the rest of the Reactive SQL clients
- Service Binding for MySQL using community operator

## Contacts

* Tester: Michal Vavřík <mvavrik@redhat.com>

## References

- [Service Binding](https://quarkus.io/guides/deploying-to-kubernetes#service_binding)
- [How to use Quarkus with the Service Binding Operator](https://developers.redhat.com/articles/2021/12/22/how-use-quarkus-service-binding-operator)
- [RHBQ 2.7 - Service Binding](https://access.redhat.com/documentation/en-us/red_hat_build_of_quarkus/quarkus-2-7/guide/4de2ab50-cb94-426d-8f6b-75ecbec52e58)
- [Service Binding Specification for Kubernetes](https://github.com/servicebinding/spec)
- [Service Binding Operator Documentation](https://redhat-developer.github.io/service-binding-operator/userguide/intro.html)
- [The Service Binding Operator](https://github.com/redhat-developer/service-binding-operator)
- [Postgres Operator from Crunchy Data](https://access.crunchydata.com/documentation/postgres-operator/v5/tutorial/)
  