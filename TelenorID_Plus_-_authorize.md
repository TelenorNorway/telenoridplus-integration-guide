## TelenorID\+ Authorize

## /authorize

The ```/authorize``` API starts a login and if needed a user registration.
This endpoint implements the [Authorize Endpoint](https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint) in the OpenID Connect Core standard.
See the [Standard OIDC Authorization Code Flow](TelenorID_Plus__-_standard_oidc_flows.md) for a sequence diagram showing the process and usage of this API.

## Input

The endpoint supports HTTP GET

| Parameter | Description | Type | Required |
| ------------- |:-------------:|:-------------:|:-------------:|
| ```client_id``` | identifier of the client | String | True |
| ```scope```	| one or more registered scopes, see more info [here](TelenorID_Plus_-_scopes.md) | String | True |
| ```redirect_uri``` | must exactly match one of the allowed preconfigured redirect URIs for that client | String | True |
| ```response_type``` | Should be set to ```code``` as standard. see more information below | String | False |
| ```response_mode``` | ```query``` (default) sends the token response as query string response,  ```form_post``` sends the token response as a form post instead of a fragment encoded redirect | String | False |
| ```state``` | TelenorID\+ will echo back the state value on the token response, this is for round tripping state between client and provider, correlating request and response and CSRF/replay protection. (recommended) | String | False |
| ```prompt``` | Values: ```none``` or ```login```, see more info below | String | False |
| ```code_challenge``` | sends the code challenge for PKCE | String | False |
| ```code_challenge_method``` | Only allowed value: ```S256``` | String | False |
| ```login_hint``` | Can be used to prefill the MSISDN | String | False |
| ```ui_locales``` | Use  value ```no``` to choose Norwegian language. | String | False |
| ```acr_values``` | Used to specify a [authentication assurance level](TelenorID_Plus_-_assurance_level.md#authentication-assurance-aal) OR specify a [authentication provider](TelenorID_Plus_-_authentication_providers.md#authentication-providers) . Should only be used if you NEED to control this.  | String | False |


__response_type__

The ```response_type``` can have one of the following values:

| Value               |                              Description                              |
|---------------------|:---------------------------------------------------------------------:|
| ```id_token```      |     requests an identity token (only identity scopes are allowed)     |
| ```token```         |      requests an access token (only resource scopes are allowed)      |
| ```id_token```      |         token requests an identity token and an access token          |
| ```code```          |                    requests an authorization code                     |
| ```code id_token``` |           requests an authorization code and identity token           |
| ```code id_token``` | token requests an authorization code, identity token and access token |

In Telenor the value ```code```  is the recommended default flow that should always be used.

## Check if user has Session

The OIDC protocol supports a ```prompt=none``` parameter on the /authorize request that allows applications to indicate that TelenorID\+ must not display any user interaction - silent authentication.  

TelenorID\+ will either return the requested response back to the application (access\_token), or return an error if the user is not already authenticated or if some type of consent or prompt is required before proceeding.  

If the user was already logged in, TelenorID\+ will respond exactly as if the user had authenticated manually through the login page.  

Be aware that some client libraries will automatically have a "fallback" to normal login, but this can largely be overridden.

If the user was not logged in via Single Sign-on (SSO) or their SSO session had expired, TelenorID\+ will redirect to the specified redirect\_uri (callback URL) with an error:

*   login\_required - The user was not logged in at TelenorID\+, so silent authentication is not possible. TelenorID\+ requires end user authentication.

If an error are returned, an /authorize request without ```prompt=none``` parameter must be sent for the end user to be redirected to the login page.


## Force authentication and ignore SSO

The OIDC protocol supports a ```prompt=login``` parameter on the /authorize request that allows applications to indicate that TelenorID\+ must authentication even if the user already has a session.
Using this parameter will always force a new login for the end-user.


## More resources

More information can be read in the [API doc for the framework used by TelenorID\+](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)