QUARKUS-4179 TP status of quarkus-oidc-token-propagation extensions

## Links to the resources
Quarkus Jira: https://issues.redhat.com/browse/QUARKUS-4179
QE Jira: https://issues.redhat.com/browse/QQE-668


## Scope of the testing
Support for quarkus-oidc-token-propagation and quarkus-oidc-token-propagation-reactive.
Functionality, that should be supported:
- Client accessing protected endpoint using token exchange grant
- Usage of AccessTokenRequestFilter and JsonWebTokenRequestFilter
- Classic and reactive

### Existing tests
- Test suite. Covers AccessTokenRequestFilter and usage of token request grant 
- Upstream
- There is a relevant quickstart `security-openid-connect-client-quickstart`, part of quickstarts jenkins jobs since 2.7 

### Automated test development
- Test suite (there is some coverage already). 
- Marete tests must check, that both of the extensions are present and are in tech preview status

### Impact on TS and resources
- Minor, several seconds will be added to the runtime due the addition of a couple of tests to the existing classes

## Links
- https://quarkus.io/guides/security-openid-connect-client-reference#token-propagation-reactive