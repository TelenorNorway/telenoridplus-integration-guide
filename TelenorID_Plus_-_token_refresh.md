# TelenorID\+ Token Refresh

Refresh tokens are typically longer-lived and can be used to request new access tokens after the shorter-lived access tokens expire.
It is only specific [client types](TelenorID_Plus_-_clienttypes.md) that can get Refresh tokens.
The client will only get a new refresh token if the SSO session still is active. 

### Refresh token lifetime
The lifetime of a refresh token is defined by the refresh policy in TelenorID\+.

The Client can, if the policy allows it, retrieve new refresh tokens through the token endpoint with ```grant_type=refresh_token```

### TelenorID\+ Refresh policies

| Policy       | Description                                                                                 | Sliding* | Lifetime | [Client type](TelenorID_Plus_-_clienttypes.md) |
|--------------|---------------------------------------------------------------------------------------------|----------|----------|------------------------------------------------|
| ```NoRefresh```    | Can't refresh, no refresh token is provided to client                                       | N/A      | N/A      | Public                                         |
| ```Confidential``` | The refresh token can only be used for 14 days, new token must be collected through a login | NO       | 14 days  | Confidential                                   |
| ```Mobile```      | The token can be refreshed and a new 90'days token will be provided                         | YES      | 90 days  | MobileApp                                      |
| ```PublicWeb```    | The token can be refreshed and a new 10 min token will be provided                          | YES      | 10 min   | PublicWithRefresh                              |
| ```Web```          | same as PublicWeb                                                                           | YES      | 10 min   | PublicWithRefresh                              |

 * *Sliding = YES → when refreshing the token, the lifetime of the refresh token will be renewed
 * *Sliding = NO → the refresh token will expire on a fixed point in time


#### Get refresh\_token

To get a refresh token, you must include the ```offline_access``` scope when you initiate an authentication request through the /authorize endpoint.

Once the user authenticates successfully, the application will be redirected to the redirect\_uri, with an authorization code . You can exchange this code with an access token using the /token endpoint.

The response should contain an access token, id token and a refresh token.

![OIDC Authorization Code Flow - Get Refresh Token](https://www.websequencediagrams.com/files/render?link=MA7p4bbC80u1UC3pNhxTgQW6VxfCslFlBOEdVh2jr9h5ejh0WPq7KydLkOTElSkc)

#### Use refresh\_token

You should only ask for a new token if the access\_token has expired or you want to refresh the claims contained in the id\_token.  
For example, it's bad practice to call the endpoint to get a new access\_token every time you call an API. 

To exchange the refresh\_token you received during authorization for a new access\_token, make a POST request to the [/token endpoint](TelenorID_Plus_-_token.md) using ```grant_type=refresh_token```

![OIDC Authorization Code Flow - Use Refresh Token](https://www.websequencediagrams.com/files/render?link=zAFGUc97cQ2vyov0ZhSA6qhSvWvBneegE8bWKv75CsGmYLQZ3lMQVcAmwCe69B84)

