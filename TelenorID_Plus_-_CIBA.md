## TelenorID\+ \(CIBA\) Client Initiated Backchannel Authentication

To start using the ciba flows there are a few requirements that have to be met ahead of time, these are as follows:
1. Client with CIBA-flow enabled.
2. This client needs to have the vipps scope enabled for it.
3. The client also has to be a confidential client.

To start a ciba request the endpoint is ```https://id-test.telenor.no/connect/ciba```. The endpoint expects a POST request with an application/x-www-form-urlencoded body.
The body should have the following content key/value entries:


| Key                    | Value                 | Explanation                                                                     |
|------------------------|-----------------------|---------------------------------------------------------------------------------|
| ```client_id```        | id of your client     | The id you were provided when you receive your client                           |
| ```client_secret```    | secret of your client | The secret you were provided when you receive your client                       |
| ```scope```            | your requested scopes | vipps is a mandatory scope for now - currently only ciba auth method            |
| ```login_hint```       | phone number          | phone number of the person you are authenticating                               |
| ```requested_expiry``` | number between 1-300  | OPTIONAL - lifetime of the ciba request, defaults to 300 and max is 300 seconds |

This request will return a JSON body:
```
{
    "auth_req_id": "43db8458-06f3-4d43-8d14-a2c257a519be",
    "expires_in": 300,
    "interval": 5
}
```
auth_req_id - you will need later to poll for the token.

expires_in - is the lifetime of the request, if it hasn't been completed in that amount of time it will be deleted and never finished.

interval - shortest amount of time between polls before getting a slow down response. I.e. if you poll every 4 seconds you will get an error response that tells your application to slow down.

Once you have these values, you need to wait the specified time in the interval before you start polling for the token. The endpoint that can be polled is ```https://id-test.telenor.no/connect/token``` which again accepts application/x-www-form-urlencoded and the key/value pairs:

| Key                 | Value                                | Explanation                                               |
|---------------------|--------------------------------------|-----------------------------------------------------------|
| ```grant_type```    | urn:openid:params:grant-type:ciba    | It will always be this value for CIBA                     |
| ```client_id```     | id of your client                    | The id you were provided when you receive your client     |
| ```client_secret``` | secret of your client                | The secret you were provided when you receive your client |
| ```auth_req_id```   | the value the first request returned | your reference to the auth context                        |

Once you start polling the token endpoint will simply return a HTTP 400 - Bad Request with the body:
```
{
    "error": "authorization_pending"
}
```

until the request is completed in vipps. Once it has been completed in vipps the polling will return the tokens.
```
{
    "id_token": <id-token-jwt>,
    "access_token": <access-token-jwt>,
    "expires_in": 3600,
    "token_type": "Bearer",
    "scope": "openid profile email phone vipps"
}
```



If for some reason the request expires before it is completed in vipps, or the id is just invalid, the endpoint will return:
```
{
    "error": "expired_token"
}
```
or
```
{
    "error": "invalid_grant"
}
```

