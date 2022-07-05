## Userinfo endpoint

The ```userinfo_endpoint```  can be used to retrieve updated identity information about a user (see [spec](http://openid.net/specs/openid-connect-core-1_0.html#UserInfo).
The information in this endpoint has overlap with the userinformation found in the [ID Token](TelenorID_Plus_-_idtokens.md).

If updated end-user information is'nt critical for your services and it's more important to reduce dependencies and latency, then you could evaluate to only using the [ID Token](TelenorID_Plus_-_idtokens.md) and ignore this userinfo endpoint.

The endpoint supports ```HTTP GET``` and ```HTTP POST```

The client must provide the [Access Token](TelenorID_Plus_-_accesstokens.md) as a Authorization bearer token to use this endpoint.
The scopes in the Access Token will decided witch claims the client is authorized to see and what claims that are returned in the response.

## Response

The userinfo endpoint has the following claims in the response.


| Parameter | Description | example |
|-----------|------------------------------------------------------------------------------|------------------------------------------|
| email | Requires a access token with scope email | ```email@email.com``` |
| phone_number | Requires a access token with scope phone | ```+4799988777``` |
| kurtid | CustomerId, Requires a access token with scope profile | ```193885119``` |
| given_name | Requires a access token with scope profile | ```Sortebill``` | 
| family_name | Requires a access token with scope profile | ```Duck``` |
| birthdate |  Requires a access token with scope profile | ```1984-12-12``` |
| analytics_uuid | A uuid connected to the user account that SHOULD be the same throughout the user lifecycle, but not managed as the kurtid. Requires a access token with scope profile | ```3a238dc1-86d5-49ce-9beb-3f8453b0cb41``` |
| ial | Identity assaurance level for the user account | ```telenor.identity.ial2``` |
| sub | “subject identifier” - an unique identifier for the authenticated user. The value is pairwise, meaning a given client will always get the same value, whilst different clients do not get equal values for the same user. | ```3ffecade-dd8f-460d-bee9-b81e8a4edae7``` |


### Example response

```
{
"email":"email@email.com",
"phone_number":"\u002B4799988777",
"kurtid":"193883119",
"given_name":"Sortebill",
"family_name":"Duck",
"birthdate":"1984-02-01",
"analytics_uuid":"3a238dd1-16d1-42ce-2beb-3f8423b0cb21",
"ial":"telenor.identity.ial2",
"sub":"3ffebade-dd8f-460d-bee9-b82e8a2fdae7"'
}
```

More information about the endpoint can be found here: [API doc for the framework used by TelenorID\+](https://identityserver4.readthedocs.io/en/latest/endpoints/userinfo.html)
