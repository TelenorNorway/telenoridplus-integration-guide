# TelenorID\+ Token endpoint

The ```token_endpoint``` is used to retrieve [ID-Token](TelenorID_Plus_-_idtokens.md), [Access Token](TelenorID_Plus_-_accesstokens.md) and in some cases Refresh Tokens. Read more about the [TelenorID\+ Token refresh](TelenorID_Plus_-_token_refresh.md).

 * The [ID-Token](TelenorID_Plus_-_idtokens.md) is used by the client application and contains information about the end-user and the authentication, 
 * the [Access Token](TelenorID_Plus_-_accesstokens.md) is used by the client application to get access to other services/API. 
 * And the [Refresh Tokens](TelenorID_Plus_-_token_refresh.md) is used to get a new valid [Access Token](TelenorID_Plus_-_accesstokens.md) through the [token refresh process](TelenorID_Plus_-_token_refresh.md).

URL's to the endpoint can be found through the [Discovery endpoint](TelenorID_Plus_-_discovery.md)

## Input

The endpoint supports HTTP POST

| Parameter | Description | Type | Required |
| ------------- |:-------------:|:-------------:|:-------------:|
| ```client_id``` | identifier of the client | String | True |
| ```grant_type``` | ```authorization_code```, ```client_credentials```,  ```refresh_token```, ```urn:ietf:params:oauth:grant-type:device_code``` | String | True |
| ```client_secret``` | Client secret either in the post body, or as a basic authentication header.  | String | False |
| ```scope```	| one or more registered scopes, see more info [here](TelenorID_Plus_-_scopes.md) | String | True |
| ```redirect_uri``` | must exactly match one of the allowed preconfigured redirect URIs for that client. required for the ```authorization_code``` grant type | String | False |
| ```code``` | the authorization code (required for ```authorization_code grant``` type). NOTE: This code is __one-time-use only__. Never try to use a authorization code more than once.  | String | False |
| ```code_verifier``` | PKCE proof key | String | False |
| ```refresh_token``` | the refresh token (required for ```refresh_token grant``` type) | String | False |
| ```device_code``` | the device code (required for ```urn:ietf:params:oauth:grant-type:device_code``` grant type) | String | False |
| ```tnuId``` | Only used when you want to do a [token exchange](TelenorID_Plus_-_token_refresh.md). Requries that ```grant_type``` is ```refresh_token``` (deprecated) | String | False |



## Example

```
POST /connect/token
CONTENT-TYPE application/x-www-form-urlencoded

    client_id=client1&
    client_secret=secret&
    grant_type=authorization_code&
    code=hdh922&
    redirect_uri=https://myapp.com/callback
```

## More resources

More information about the endpoint can be found here:
 - [API doc for the framework used by TelenorID\+](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)
 - [OpenID core specification](https://openid.net/specs/openid-connect-core-1_0.html#TokenRequest)