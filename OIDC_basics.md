## Overview OpenID Connect (OIDC)


## Introduction to tokens
There are three types of tokens in the [OpenID connect standard](https://openid.net/specs/openid-connect-core-1_0.html), two of them is defined by the the [OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749#section-1.4) and the last one is defined by [OpenID connect standard](https://openid.net/specs/openid-connect-core-1_0.html) it self.

| Token type | Description | Ref |
| ------------------------- | -------------------- | -------------------- |
| Refresh Token | Refresh tokens are credentials used to obtain access tokens. Note that a refresh will fail if the end user has terminated their session (logged out) | [The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749#section-1.4) |
| ID Token | security token that contains Claims about the Authentication of an End-User. What claims are available in the id_token is dependent on which scopes are requested by your client. - do **not** use ID tokens to gain access to an API | [OpenID connect standard](https://openid.net/specs/openid-connect-core-1_0.html) | 



### Simplified OpenID Connect Authorization Code Flow

![OIDC Authorization Code Flow](images/IntegrationGuide_AuthCodeFlow-simplified.png)


### Token Refresh

Refresh tokens are typically longer-lived and can be used to request new access tokens after the shorter-lived access tokens expire. Refresh tokens are often used in native applications on mobile devices in conjunction with short-lived access tokens to provide seamless UX without having to issue long-lived access tokens.

The refresh token is stored in session. Then, when a session needs to be refreshed (for example, a preconfigured timeframe has passed or the user tries to perform a sensitive operation), the client uses the refresh token on the backend to obtain a new set of tokens, using the /token endpoint with _grant\_type=refresh\_token_.

#### Get refresh\_token

To get a refresh token, you must include the _**offline\_access**_ scope when you initiate an authentication request through the /authorize endpoint.

Once the user authenticates successfully, the application will be redirected to the redirect\_uri, with an authorization code . You can exchange this code with an access token using the /token endpoint.

The response should contain an access token, id token and a refresh token.

![OIDC Authorization Code Flow - Get Refresh Token](https://www.websequencediagrams.com/files/render?link=MA7p4bbC80u1UC3pNhxTgQW6VxfCslFlBOEdVh2jr9h5ejh0WPq7KydLkOTElSkc)

#### Use refresh\_token

You should only ask for a new token if the access\_token has expired or you want to refresh the claims contained in the id\_token.  
For example, it's bad practice to call the endpoint to get a new access\_token every time you call an API. 

To exchange the refresh\_token you received during authorization for a new access\_token, make a POST request to the /token endpoint using _**grant\_type=refresh\_token**_

![OIDC Authorization Code Flow - Use Refresh Token](https://www.websequencediagrams.com/files/render?link=zAFGUc97cQ2vyov0ZhSA6qhSvWvBneegE8bWKv75CsGmYLQZ3lMQVcAmwCe69B84)


## Definitions


| Term | Description |
| ------------------------- | -------------------- |
| Resource Owner/End User   | The User that has access to the resource and the entity capable of granting access to the protected resource |
| Client                    | An application making protected resources request on behalf of the resource owner/end user and with the resource owner's authorization |
|                           | The application that needs access to the protected resource |
| Authorisation Server (AS) / Identity Provider (IDP)  | The system whom you delegate your applications trust to provide authentication- and authorisation information about an end user or system. For Telenor Norway this system is TelenorID\+. TelenorID\+ is a federated gateway and delegates authentication of end users to other Identity Providers (e.g. Telenor Digital's "Connect"). |
| Flows                     | While the oauth protocol originally contained numerous flows. All but two are now considered insecure and not recommended. The two remaining flows are "authorisation code with PKCE" and "authorisation code with client secret". |
|                           | The PKCE (Proof-key for Code-Exchange) flow is the default and recommended for all **public** clients. A PKCE allows the Authorisation server to validate that client who receives tokens is the same client that requested them (i.e solving the problem of authorisation code request being vulnerable to hijacks). |
|                           | Client secrets are only permitted for clients who will run on back-end servers. These clients typically need to access broader scopes than those running on clients (front-end / mobile apps). This is the recommended flow for **confidential** web clients (with BFFs) |