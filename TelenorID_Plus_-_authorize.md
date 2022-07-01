## TelenorID\+ Authorize

## /authorize

The ```/authorize``` API starts a login and if needed a user registration.


## Input

The endpoint supports HTTP GET

| Parameter | Description | Type | Required |
| ------------- |:-------------:|:-------------:|:-------------:|
| ```client_id``` | identifier of the client | String | True |
| ```scope```	| one or more registered scopes, see more info [here](TelenorID_Plus_-_scopes.md) | String | True |
| ```redirect_uri``` | must exactly match one of the allowed preconfigured redirect URIs for that client | String | True |
| ```response_type``` | see more information below | String | False |
| ```response_mode``` | ```query``` (default) sends the token response as query string response,  ```form_post``` sends the token response as a form post instead of a fragment encoded redirect | String | False |
| ```state``` | TelenorID\+ will echo back the state value on the token response, this is for round tripping state between client and provider, correlating request and response and CSRF/replay protection. (recommended) | String | False |
| ```prompt``` | Values: ```none``` or ```login```, see more info below | String | False |
| ```code_challenge``` | sends the code challenge for PKCE | String | False |
| ```code_challenge_method``` | Only allowed value: ```S256``` | String | False |
| ```login_hint``` | Can be used to prefill the MSISDN | String | False |
| ```ui_locales``` | Use  value ```no``` to choose Norwegian language. | String | False |
| ```acr_values``` | Use  value ```urn:tnidplus:kyc``` to start BankID login, see more information [here](TelenorID_Plus_-_kyc_bankid_-_integration_example_step_by_step.md). Use prefix ```idp:``` if you need a login with a specific identity provider. The  name for the different identity providers are unique for each ```client_id```. please take contact with us to get your relevant identity provider names.  | String | False |
| ```context``` | Default ```login``` Use value ```kyc``` to get a ID\+ customized for confirmation of identity and not authentication | String | False |


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