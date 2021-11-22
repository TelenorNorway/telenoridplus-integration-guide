## Overview OpenID Connect (OIDC)



### Core Concepts

OIDC is a superset of the Oauth protocol. If you've used OAuth (e.g via Apigee), OIDC should be very familiar.

|                           |                      |
| ------------------------- | -------------------- |
| Resource Owner/End User   | The User that has access to the resource and the entity capable of granting access to the protected resource |
| Client                    | An application making protected resources request on behalf of the resource owner/end user and with the resource owner's authorization |
|                           | The application that needs access to the protected resource |
| Authorisation Server (AS) / Identity Provider (IDP)  | The system whom you delegate your applications trust to provide authentication- and authorisation information about an end user or system. For Telenor Norway this system is "IBIS". IBIS is a federated gateway and delegates authentication of end users to other Identity Providers (e.g. Telenor Digital's "Connect"). |
| id\_token                 | A JSON web token (JWT) containing claims about the end user who logged in, typically their name and address. What claims are available in the id\_token is dependent on which scopes are requested by your client. |
|                           | _**Note**_: Unlike the access\_token, the id\_token shall not be sent as a proof of authentication. The id\_token is solely designed to be consumed (used) on the receiving client (the token contains an "issued to" claim which must be validated; "is this token issued to me?") - do **not** use ID tokens to gain access to an API |
| access\_token             | The token used for authorisation purposes, e.g. "can this client access this API?". The access\_tokens issued by IBIS can also contain identifiers for the current end user, e.g "KurtId" and/or "TnuId". |
|                           | The access token has short expiration time and will have to be refreshed frequently: The OAuth protocol does not contain the concept of "log out", instead it relies on all sessions being short lived. When a end user wants to log out we simply stop to refresh the access token. |
| refresh\_token            | As the name implies this token is used to refresh (prolong) the end user's session. The refresh\_token is exchanged at the auth. server which will issue a new access and refresh\_token. Note that a refresh will fail if the end user has terminated their session (logged out). |
| Flows                     | While the oauth protocol originally contained numerous flows. All but two are now considered insecure and not recommended. The two remaining flows are "authorisation code with PKCE" and "authorisation code with client secret". |
|                           | The PKCE (Proof-key for Code-Exchange) flow is the default and recommended for all **public** clients. A PKCE allows the Authorisation server to validate that client who receives tokens is the same client that requested them (i.e solving the problem of authorisation code request being vulnerable to hijacks). |
|                           | Client secrets are only permitted for clients who will run on back-end servers. These clients typically need to access broader scopes than those running on clients (front-end / mobile apps). This is the recommended flow for **confidential** web clients (with BFFs) |

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