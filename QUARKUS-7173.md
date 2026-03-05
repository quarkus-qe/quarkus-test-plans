# QUARKUS-7173 - Hibernate Spatial - Technology preview support

JIRA: https://issues.redhat.com/browse/QUARKUS-7173

Upstream PR: https://github.com/quarkusio/quarkus/pull/51586

Hibernate Spatial enables applications to store and query geometric data, such as geographic coordinates or shapes representing areas and paths.
Typical spatial data include:
- `Point` – a specific location (for example GPS coordinates)
- `LineString` – a path or route defined by multiple points
- `Polygon` – an area defined by a closed set of coordinates

Using these geometry types applications can perform spatial operations directly in database queries, such as:
- checking whether a location lies inside a region
- determining whether two geometries intersect
- calculating the distance between locations

Hibernate Spatial integrates these capabilities with Hibernate ORM and allows spatial predicates and functions to be used in JPQL/HQL queries.

Quarkus provides integration with two supported geometry libraries: `Java Topology Suite (JTS)` and `Geolatte`.
Both libraries provide geometry models that can be mapped to spatial database columns and used in spatial queries.

## Existing test coverage
Upstream tests focus on verifying the core Hibernate ORM Spatial functionality:
- mapping of spatial geometry types to database columns
- persistence and retrieval of geometry values
- usage of spatial predicates and functions in HQL
- compatibility with `JTS` and `Geolatte` geometry models

## Scope of the testing
Testing will focus on verifying that Hibernate Spatial works correctly when used in a Quarkus application with supported geometry libraries and spatial databases.

### Spatial entity mapping:
- Verify that spatial entity attributes using `JTS` and `Geolatte` geometry types can be persisted and retrieved in a Quarkus application
- Verify that spatial entities loaded through SQL import scripts are correctly mapped and accessible through Hibernate ORM

### Spatial queries:
Verify that spatial queries executed through JPQL behave correctly and return expected geometry results.
This includes validating common spatial operations such as spatial predicates and distance-based queries when working with geometric data.

### Geometry handling:
- Verify correct mapping and retrieval of different geometry types such as points, paths, and polygons
- Verify that geometry values can be converted to textual representation for validation purposes

### Supported and tested databases
Spatial functionality will be validated against some supported spatial databases: PostGIS and MySQL

## Impact on test suites and testing automation
The `hibernate/hibernate-orm` module in the Test Suite will be extended with new spatial integration tests covering the scenarios above.

## Impact on resources
The expected execution time will be increased about 1,5 minutes for JVM mode and 6 minutes for native mode.
On OCP, the execution time is expected to increase by around 4 minutes for JVM mode and 8 minutes for native mode.

## Getting familiar with the feature
- [Hibernate Spatial documentation](https://docs.hibernate.org/orm/current/userguide/html_single/#spatial)
- [Quarkus Hibernate ORM spatial guide](https://quarkus.io/guides/hibernate-orm#spatial)
- [Quarkus upstream PR implementing the feature](https://github.com/quarkusio/quarkus/pull/51586)

## Contacts
* Tester: Georgii Troitskii <gtroitsk@ibm.com>

## References
- [Hibernate Spatial documentation](https://docs.hibernate.org/orm/current/userguide/html_single/#spatial)
- [Quarkus Hibernate ORM spatial guide](https://quarkus.io/guides/hibernate-orm#spatial)
- [Quarkus upstream PR implementing the feature](https://github.com/quarkusio/quarkus/pull/51586)
