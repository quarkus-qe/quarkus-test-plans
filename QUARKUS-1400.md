# QUARKUS-1400 Operator SDK product support (Tech preview)

Jira: https://issues.redhat.com/browse/QUARKUS-1400

The goal of this feature is to provide basic SDK for implementation Kubernetes operators with RHBQ as a part of internal
midstream.

## Scope of the testing

### Impact on test suites and testing automation
* For Elektra, none. Operator SDK in RHBQ will be part of internal midstream aimed at managed applications layered
  products.

### Impact on resources
* The SDK will be tested by layered products and the midstream release will have its own schedule (midstream).

## Future considerations
* Midstream is not a standard situation. Eventually there may be a plan to include the Operator SDK support as a part
  of RHBQ. In such case, the following will have to be considered:
  * Stability of Kubernetes client used in RHBQ. There previously have been issues with compatibility between minor and
    micro-releases.
  * The API that RHBQ will be providing and its stability.
  * Ease of use in native mode. Native mode would be the end-post in regard to the fact that the Kube operators have to
    boot fast and be rather austere with resources.
  * Documentation.

## Contacts
* Tester: Michal Jurč <mjurc@redhat.com>

## References
* Feature request: [QUARKUS-1400 Operator SDK product support (Tech preview)](https://issues.redhat.com/browse/QUARKUS-1400)