# Readme - gjo notes
* [pivotal-springone implementing Microserives with spring sec 5.2](https://www.youtube.com/watch?v=JnYIsvJY7gM&feature=youtu.be)

joe grandja & stephen doxsee

## standards
* OAuth 2.0 Authorization Frmwk
* OpenID Connect Core 1.0
* OpenID Connect Discovery
* JWT - JSON Web Token
* JWS - JSON Web Signature

## components

* Keycloak
* ui app
* microservice a,b,c

## App

* [ui app](localhost:8080)

uiapp -> svc-a, service-b 

service-b -> service-c (w/ client cred)

### ui-app

* depdencies
spring-boot-starter-security

spring-boot-starter-oauth2-client

* application.yml

spring:/security:/oauth2:/client:/registration:

```
login-client:
  provider: keycloak
  client-id: login-client
```

#### @ControllerAdvice public class DefaultControllerAdvice

*   -  current user
```java
	@ModelAttribute("currentUser")
	UserModel currentUser(OAuth2AuthenticationToken oauth2Authentication) {
		UserModel currentUser = new UserModel();
		if (oauth2Authentication != null) {
			OidcUser oidcUser = (OidcUser) oauth2Authentication.getPrincipal();
			currentUser.setUserId(oidcUser.getSubject());
			currentUser.setFirstName(oidcUser.getGivenName());
			currentUser.setLastName(oidcUser.getFamilyName());
			currentUser.setEmail(oidcUser.getEmail());
		}
		return currentUser;
	}
```

*   -  token claims
```java	

	@ModelAttribute("idTokenClaims")
	Map<String, Object> idTokenClaims(OAuth2AuthenticationToken oauth2Authentication) {
		if (oauth2Authentication == null) {
			return Collections.emptyMap();
		}
		OidcUser oidcUser = (OidcUser) oauth2Authentication.getPrincipal();
		final List<String> claimNames = Arrays.asList("iss", "sub", "aud", "azp", "given_name", "family_name", "email");
		return oidcUser.getClaims().entrySet().stream()
				.filter(e -> claimNames.contains(e.getKey()))
				.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
	}
```

#### Tokens

* access token
* id token  
  - view with https://jwt.io
* refresh token

- Discovery endpoint

### microservice a 
secure
* build.gradle - dependencies
`spring-boot-starter-oauth2-resource-server`
* application.yml - spring security
```
oauth2:
  resourceserver:
    jwt:
	  issuer-uri: ...
	  
```

