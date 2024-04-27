# OAuth 2.0
[FirstSteps](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity?view=aspnetcore-7.0&tabs=visual-studio)
Skeleton is created with `Register`, `Login`, `LogOut`, `RegisterConfirmation` files. The confirmation email is not sent, there is a dummy confirmation though.
Google authentication is implemented, for now, only for one test account, with my email. The docs said i might have to register the client as an app in the playstore to actually use this as a way of authentication in live.

`dotnet user-secrets set "Authentication:Github:ClientId" "<client-id>" 
`dotnet user-secrets set "Authentication:Github:ClientSecret" "<client-secret>"

[RFC 6749: The OAuth 2.0 Authorization Framework (rfc-editor.org)](https://www.rfc-editor.org/rfc/rfc6749#page-4)

>For example, an end-user (resource owner) can grant a service (client) access to her protected photos stored at a photo-sharing service (resource server), without sharing her username and password with he printing service.  Instead, she authenticates directly with a server trusted by the photo-sharing ervice (authorization server), which issues the printing service delegation-specific credentials (access token).

## Roles
 
   resource owner
      An entity capable of granting access to a protected resource.
      When the resource owner is a person, it is referred to as an
      end-user.

   resource server
      The server hosting the protected resources, capable of accepting
      and responding to protected resource requests using access tokens.

   client
      An application making protected resource requests on behalf of the
      resource owner and with its authorization.  The term "client" does
      not imply any particular implementation characteristics (e.g.,
      whether the application executes on a server, a desktop, or other
      devices).

   authorization server
      The server issuing access tokens to the client after successfully
      authenticating the resource owner and obtaining authorization.

## Control flow
	+--------+                               +---------------+
	|        |--(A)- Authorization Request ->|   Resource    |
	|        |                               |     Owner     |
	|        |<-(B)-- Authorization Grant ---|               |
	|        |                               +---------------+
	|        |
	|        |                               +---------------+
	|        |--(C)-- Authorization Grant -->| Authorization |
	| Client |                               |     Server    |
	|        |<-(D)----- Access Token -------|               |
	|        |                               +---------------+
	|        |
	|        |                               +---------------+
	|        |--(E)----- Access Token ------>|    Resource   |
	|        |                               |     Server    |
	|        |<-(F)--- Protected Resource ---|               |
	+--------+                               +---------------+

					   
   (A)  The client requests authorization from the resource owner.  The
        authorization request can be made directly to the resource owner
        (as shown), or preferably indirectly via the authorization
        server as an intermediary.

   (B)  The client receives an authorization grant, which is a
        credential representing the resource owner's authorization,
        expressed using one of four grant types defined in this
        specification or using an extension grant type.  The
        authorization grant type depends on the method used by the
        client to request authorization and the types supported by the
        authorization server.

   (C)  The client requests an access token by authenticating with the
        authorization server and presenting the authorization grant.

   (D)  The authorization server authenticates the client and validates
        the authorization grant, and if valid, issues an access token.

---

   (E)  The client requests the protected resource from the resource
        server and authenticates by presenting the access token.

   (F)  The resource server validates the access token, and if valid,
        serves the request.

## Refresh Token
	  +--------+                                           +---------------+
	  |        |--(A)------- Authorization Grant --------->|               |
	  |        |                                           |               |
	  |        |<-(B)----------- Access Token -------------|               |
	  |        |               & Refresh Token             |               |
	  |        |                                           |               |
	  |        |                            +----------+   |               |
	  |        |--(C)---- Access Token ---->|          |   |               |
	  |        |                            |          |   |               |
	  |        |<-(D)- Protected Resource --| Resource |   | Authorization |
	  | Client |                            |  Server  |   |     Server    |
	  |        |--(E)---- Access Token ---->|          |   |               |
	  |        |                            |          |   |               |
	  |        |<-(F)- Invalid Token Error -|          |   |               |
	  |        |                            +----------+   |               |
	  |        |                                           |               |
	  |        |--(G)----------- Refresh Token ----------->|               |
	  |        |                                           |               |
	  |        |<-(H)----------- Access Token -------------|               |
	  +--------+           & Optional Refresh Token        +---------------+

   (A)  The client requests an access token by authenticating with the
        authorization server and presenting an authorization grant.

   (B)  The authorization server authenticates the client and validates
        the authorization grant, and if valid, issues an access token
        and a refresh token.

   (C)  The client makes a protected resource request to the resource
        server by presenting the access token.

   (D)  The resource server validates the access token, and if valid,
        serves the request.

   (E)  Steps (C) and (D) repeat until the access token expires.  If the
        client knows the access token expired, it skips to step (G);
        otherwise, it makes another protected resource request.

   (F)  Since the access token is invalid, the resource server returns
        an invalid token error.

   (G)  The client requests a new access token by authenticating with
        the authorization server and presenting the refresh token.  The
        client authentication requirements are based on the client type
        and on the authorization server policies.

   (H)  The authorization server authenticates the client and validates
        the refresh token, and if valid, issues a new access token (and,
        optionally, a new refresh token).


### Tokens

[Tokens](https://auth0.com/docs/secure/tokens)
[Decoding JSON Web Tokens](https://jwt.io/)
-   **JOSE Header**: contains metadata about the type of token and the cryptographic algorithms used to secure its contents.
-   **JWS payload** (set of [claims](https://tools.ietf.org/html/rfc7519#section-4)): contains verifiable security statements, such as the identity of the user and the permissions they are allowed.
-   **JWS signature**: used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you **must** [check its signature](https://auth0.com/docs/secure/tokens/json-web-tokens/validate-json-web-tokens) before storing and using it.

The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.