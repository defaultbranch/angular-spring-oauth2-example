# OAuth2 Example with Angular Frontend and Spring Backend

This demo project implements application support for the Authorization
Code Grant as specified in [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1),
for a (Kubernetes) web application consisting of a SPA frontend and a server backend.


## Implementation Goals

- allow for a list of one or more external authentication providers
  (Keycloak, Azure Active Directory, …), configurable in the backend
  and then directly usable by the frontend
- frontend users pick their authentication provider from the list,
  and authenticate with that authentication provider
- upon success, the backend issues a session token to the frontend,
  containing the confirmed identity
- _only_ the backend session token is given to the frontend; tokens
  issued by the authentication provider (access, refresh) are not
  exposed to the frontend application


## Conceptual Goals

- get a clear view on the code it takes to implement the Authorization Code Grant flow,
- provide a concise but working implementation that can be used for testing or debugging particular OAuth2 identity providers,
- get a starting point for future, refined implementations.


## Choice of Languages, Frameworks, and 3rd-Party Libraries

- Backend ("Client" in RFC 6749 terminology) is a Spring app written in Kotlin
- Browser frontend ("Client" in RFC 6749 terminology) is an Angular app
- Both will run as Kubernetes containers behind an Nginx load balancer


## Project Structure

- [./backend](./backend) contains one Maven project
  - providing the RFC 6749 `authorization_code` endpoint
  - providing a REST interface for the frontend
    - supplying the list of accepted authorization providers ("Authorization Server" in RFC 6749 terminology)
    - supplying session tokens
- [./frontend](./frontend) contains one Angular project
  - informing the user about her login status
  - if not logged int, providing login choices
  - if logged in, providing logout
- [./containers](./containers) contains the `Dockerfile`s required for containerization
- [./devops](./devops) contains Kubernetes and Terraform code for roll-out
  - also contains the ingress-nginx configuration for the application
