## TelenorID\+ \(CIBA\) Client Initiated Backchannel Authentication

This document describes CIBA for both [Vipps](#vipps) and [BankID](#bankid). The flows differ in how to initiate a CIBA request, but the token polling is the same for both.


### Setup

To start using the ciba flow there are a few requirements that have to be met ahead of time, these are as follows:
1. Client with CIBA-flow enabled.
2. This client needs to have either the vipps and phone scopes enabled for it or the bankid scope. These are mandatory scopes for the CIBA flow.
3. The client also has to be a confidential client. 

(NOTE: Enabling CIBA flow will disable the normal front-channel flow. Meaning you cannot reuse the same client for both CIBA and normal OIDC flows.)

### Vipps
#### Initiating a request
To start a vipps ciba request the endpoint is ```https://id-test.telenor.no/connect/ciba```. The endpoint expects a POST request with an application/x-www-form-urlencoded body.
The body should have the following content key/value entries:

| Key                    | Value                 | Explanation                                                                       |
|------------------------|-----------------------|-----------------------------------------------------------------------------------|
| ```client_id```        | id of your client     | The id you were provided when you receive your client                             |
| ```client_secret```    | secret of your client | The secret you were provided when you receive your client                         |
| ```scope```            | your requested scopes | vipps and phone are mandatory for the vipps flow. This is a space-separated list. |
| ```login_hint```       | phone number          | phone number of the person you are authenticating                                 |
| ```requested_expiry``` | number between 1-300  | OPTIONAL - lifetime of the ciba request, defaults to 300 and max is 300 seconds   |

(NOTE: It is also valid to send client credentials as a basic auth header and omit them from the body.)

(NOTE2: "openid" is assumed to be among your requested scopes as it is mandatory in OIDC. "profile" is also necessary for basic user information.)

### BankId
#### Initiating a request
For bankid you have to do some preparations before you send the actual CIBA request.
The first step is to send a request towards the endpoint ```https://id-test.telenor.no/api/ciba/ciba_prepare_bankid```.
This endpoint expects a POST request with an application/x-www-form-urlencoded body and the client credentials in the `Authorization` header as basic auth.
The body should have the following content key/value entries:

| Key                   | Value     | Explanation                                                 |
|-----------------------|-----------|-------------------------------------------------------------|
| ```kurt_id```         | a kurt id | This is the kurt id of the person you want to authenticate. |

This request will return a JSON body:
```
{
    "login_hint_token": <login-hint-token-jwt>,
    "binding_message": <binding-message>
}
```

login_hint_token - you will need this token to send in the actual ciba request.\
binding_message - this is a message that will be shown to the user in the bankid app. you need this for the ciba request.

Now you can send the actual ciba request towards the endpoint ```https://id-test.telenor.no/connect/ciba```.
This endpoint expects a POST request with an application/x-www-form-urlencoded body. And the same `Authorization` header as before.
It is also valid to send the client credentials in the body instead of the header.
The body should have the following content key/value entries:

| Key                    | Value                                 | Explanation                                                                      |
|------------------------|---------------------------------------|----------------------------------------------------------------------------------|
| ```scope```            | your requested scopes                 | bankid is a mandatory scope for the bankid flow. This is a space-separated list. |
| ```login_hint_token``` | the token you just received           | The login_hint_token you received from the prepare request.                      |
| ```binding_message```  | the binding_message you just received | The binding_message you received from the prepare request.                       |
| ```requested_expiry``` | number between 1-300                  | OPTIONAL - lifetime of the ciba request, defaults to 300 and max is 300 seconds  |

The next steps are the same for both vipps and bankid. And described below.

### Fetching the token
Once you have sent the initial request towards the ciba endpoint, following either the vipps or bankid steps, you will receive a response back.\
This response will contain a JSON body:
```
{
    "auth_req_id": "43db8458-06f3-4d43-8d14-a2c257a519be",
    "expires_in": 300,
    "interval": 5
}
```

`auth_req_id` - you will need later to poll for the token.\
`expires_in`  - is the lifetime of the request, if it hasn't been completed in that amount of time it will be deleted and never finished.\
`interval`    - shortest amount of time between polls before getting a slow down response. I.e. if you poll every 4 seconds you will get an error response that tells your application to slow down.

Once you have these values, you need to wait the specified time in the interval before you start polling for the token. The endpoint that can be polled is ```https://id-test.telenor.no/connect/token``` which again accepts application/x-www-form-urlencoded and the key/value pairs:

| Key                 | Value                                | Explanation                                               |
|---------------------|--------------------------------------|-----------------------------------------------------------|
| ```grant_type```    | urn:openid:params:grant-type:ciba    | It will always be this value for CIBA                     |
| ```client_id```     | id of your client                    | The id you were provided when you receive your client     |
| ```client_secret``` | secret of your client                | The secret you were provided when you receive your client |
| ```auth_req_id```   | the value the first request returned | your reference to the auth context                        |
(NOTE: It is also valid to send client credentials as a basic auth header and omit them from the body.)

Once you start polling the token endpoint will simply return a HTTP 400 - Bad Request with the body:
```
{
    "error": "authorization_pending"
}
```

until the request is completed by the user. Once it has been completed the polling will return the tokens.
A notable distinction between the vipps and bankid flows here is that the vipps flow is simply the user approving it in the app,
while the bankid flow requires whoever is triggering the authentication to inform the recipient of the authentication request of the `binding_message`.
```
{
    "id_token": <id-token-jwt>,
    "access_token": <access-token-jwt>,
    "expires_in": 3600,
    "token_type": "Bearer",
    "scope": "openid profile email phone vipps"
}
```



If for some reason the request expires before the user completes it, or the id is just invalid, the endpoint will return:
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
This could happen for example if the client is polling too slowly and the request expires before completion.
The user explicitly denying the request will also result in an invalid_grant error. The user failing to select the correct `binding_message` in the bankid app will also lead to an invalid_grant error.
And any communication issues that lead to the request not being found will also lead to an invalid_grant error.

Currently the lack of direct feedback on the initial request is a technical limitation we are aware of. 
It will be solved in the future with the Telenor ID and Idplus consolidation.
