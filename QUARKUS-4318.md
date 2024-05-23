QUARKUS-4318 Enhance OIDC token propagation filters to select named OIDC clients

## Links to the resources
Quarkus Jira: https://issues.redhat.com/browse/QUARKUS-4318
QE Jira: https://issues.redhat.com/browse/QQE-670


## Scope of the testing
Ability to tie REST client with OIDC client for token propagation in filters.
Functionality, that should be supported:
- An ability for rest clients to have name of OIDC client as parameter. This client should be used for token exchange. 
- Coverage in both classic and reactive mode.

### Existing tests
- Upstream covers that in https://github.com/quarkusio/quarkus/pull/39132 

### Automated test development
- Not required

### Impact on TS and resources
- None

## Links
- https://quarkus.io/guides/security-openid-connect-client-reference