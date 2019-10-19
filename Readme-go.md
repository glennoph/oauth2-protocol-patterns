# Readme gjo notes
See [youtube presentation](https://www.youtube.com/watch?v=nrmQH5SqraA)
* Implementing Microservices Security Patterns & Protocols by Joe Grandja, Adib Saikali
Also https://www.youtube.com/watch?v=JnYIsvJY7gM&feature=youtu.be 

## Overview
* each application uses OpenID Connect Server for login
* OpenID Connect Server has ui, code, db (dft h2) -- UAA
* apps do not see credentials
* impl with Spring Security v5.2

## UAA OpenID Connect Server
* Options: uaa from cloud foundary, keycloak
* spring based
* no admin console, has cli 
* designed to be imbedded in application

### launch via gradle
`./gradlew -b uaa-server/build.gradle uaa`

### ui login
`http://localhost:8090/uaa`

### config
uaa-server/uaa.yml

* dft db is h2
* list of `users`

* runs on tomcat
* uaa war file from maven central

## Configure Client Application
### Steps OIDC Provider
* register client with uaa (provider)
* get client id/secret from uaa
* uaa endpoints -- `redirect-uri` 
### Steps Client App
* add dependencies: 
  - spring-boot-starter-security
  - spring-boot-starter-oauth2-client
* update application.yml
  - app with oidc provider
  - client to user oidc provider
  
  
## OAuth Concepts
> Resource Server 
  
web or api service on network
  
> Resource Owner

usually a person who can consent to provide access to resource

* Client

an application making requests to resource server

* Authorization Server

asks resource owner if they will allow a client access to resource server
issues access tokens 


## OpenID Connect
* Authentication protocol w/ OAuth2, JWT, TLS
* Def Standard user ident token - JWT with req fields
* Def userinfo endpoint to get user detains 
  * email addr, contact info, ...
  
### Req ID Token Fields
* iss - who issues token
* iat - time token was issued
* exp - time token expires
* sub - subject unique id
* aud - audience for token 

## Spring Security 5 Classes for OAuth2/OIDC

* `ClientRegistration`

has metadata of client registered at provider

* `ClientRegistrationRepository`

* `OAuth2AuthorizedClient`

has state req for client to call protected resource

* `OAuth2AuthorizedClientRepository`



## Secure 1 Microservice
* Protocol : http (rest, soap), amqp, apache thrift, ...
* Security Token format
  * kerberos ticket 
  * saml token (binary)
  * jwt token (json)
  
### JWT tokens
in controller access `JwtAuthenicationToken`
and methods: 
* jwtAuthentication.getToken().getId()
* also getSubject() getAudience(), getAuthorities()
