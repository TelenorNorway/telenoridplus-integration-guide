## Request

To request an ID Token, use the /token endpoint.

## The ID Token

The ID Token is primary extension that OpenID Connect makes to the oAuth standard.
The ID Token is a security token that contains Claims about the End-User identity.

The [userinfo endpoint](TelenorID_Plus_-_userinfo.md) can be used to retrieve updated identity information about the end-user.
Make sure to use the [userinfo endpoint](TelenorID_Plus_-_userinfo.md) if it's important for you to have updated and correct end-user information during the user session.

If updated end-user information is'nt critical for your services and it's more important to reduce dependencies and latency, then you could evaluate to only using the ID Token and ignore the [userinfo endpoint](TelenorID_Plus_-_userinfo.md)

**NOTE:** The end-user information made available through this token is only connected to the end-user authentication. Please make sure to use the correct API if you would like to retrieve adresses, contact information or other end-user profil information. See more information about relevant API's at the [developer portal](https://developer.corp.telenor.no/).


The token is a JWT with the following structure:

### ID token header:

| Claim                      | Value                                                                                                                                                                                               |
|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```kid``` “Key identifier” | unique identifier for the key and certificate used by TelenorID\+. The corresponding public key and the certificate must be fetched from our [.well-known endpoint.](TelenorID_Plus_-_discovery.md) |
| ```alg``` “algorithm”      | algorithm used for signing the token. TelenorID\+ only supports ```RS256``` (RSA-SHA256)                                                                                                            |
| ```typ``` "Token type"     | always: ```jwt```, describing that this is a JSON Web Token                                                                                                                                         |

### ID token body:

| Claim                    | Value                                                                                                                                                                                                                     | Example                                          |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| ```iat```                | Timestamp when this token was issued.                                                                                                                                                                                     | 1639039167                                       |
| ```nbf```                | Not before - same time as ```iat```                                                                                                                                                                                       | 1639039167                                       |
| ```exp```                | Expire  - same time as ```iat```                                                                                                                                                                                          | 1639039167                                       |
| ```iss```                | The identifier of TelenorID\+ as can be verified on the [.well-known endpoint.](TelenorID_Plus_-_discovery.md)                                                                                                            | https://id-test.telenor.no                       |
| ```aud```                | The client_id of the client who received this token.                                                                                                                                                                      | tnn-mbn-android-test                             |
| ```sub```                | “subject identifier” - an unique identifier for the authenticated user. The value is pairwise, meaning a given client will always get the same value, whilst different clients do not get equal values for the same user. | 2b424013-971b-4435-bdc1-d1075b05d0e9             |
| ```idp```                |                                                                                                                                                                                                                           | no.telenor.id.proxy.tnn-mbn-android-test         |
| ```sid```                |                                                                                                                                                                                                                           | 70A6B33AB0D906BC3D203AE946CCC63B                 |
| ```amr```                | Extended [Authentication Methods References](https://datatracker.ietf.org/doc/html/rfc8176)                                                                                                                               | [See more information below](#authentication-methods-referencesamr) |
| ```ibis.sid```           | Internal session id in TelenorID\+                                                                                                                                                                                        | cf2b1147-c2e7-4c74-a32e-3afdb034e181             |
| ```analytics_uuid```      | Internal session id in TelenorID\+                                                                                                                                                                                        | 7568fe50-217f-4ab8-a4e4-29cbe7e118c9             |
| ```family_name```        | Lastname of end-user                                                                                                                                                                                                      | Rasmussen                                        |
| ```given_name```         | First and possible midle name for end-user                                                                                                                                                                                | Tom Peter                                        |
| ```preferred_username``` | The username used by the end-user during this session, can be a phonenumber or a email adress.                                                                                                                                                                                        | 4799966634                                       |


## Authentication Methods References(AMR)

 In accordance with the [OpenID Connect Core spec](https://openid.net/specs/openid-connect-core-1_0.html), section 2, the "amr" claim contains values identifying the authentication methods used in the authentication of the user. Most of the values are taken direcly from the authentication providers, such as TelenorID or Azure.

 The following values are valid, new attributes can be added if the authentication providers expand their methods:

 | Authentication provider | prefix | Values | Description | 
  |--------------------------------------------------|--------------------------------------------------|--------------------------------------------------|--------------------------------------------------| 
 | Telenor Digital TelenorID  |  ```urn:telenor.identity.amr.td_``` | ```pwd```, ```otp```, ```sso```, ```ok```, ```hdr``` | https://docs.telenordigital.com/connect/id/id_token.html |
 |Microsoft Azure | ```urn:telenor.identity.amr.az_``` | ```pwd```, ```mfa```, ```otp```, ```rsa```, ```fed```, ```wia```, ```ngcmfa```, ```wiaormfa```, ```none``` | https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens |
 | BankID | ```urn:telenor.identity.amr.bankid``` | no values | always only ```urn:telenor.identity.amr.bankid``` |
 | BankID legacy impl | ```urn:tnidplus:kyc``` | no values | This is a deprecated legacy authentication method in TelenorID\+ only here for historic reasons | 
 | others |  ```urn:tnidplus:std``` | no values | TelenorID\+ has some other authentication providers and methods, they are mostly legacy and all default to the same value |




## Example

```
eyJhbGciOiJSUzI1NiIsImtpZCI6ImNmZmIzNTJlLTJhNTAtNGJkYi1hNTUzLTk4MmZkNTNiNDBiMSIsInR5cCI6IkpXVCJ9.eyJuYmYiOjE2NTY1ODY1NjksImV4cCI6MTY1NjU4Njg2OSwiaXNzIjoiaHR0cHM6Ly9pZC10ZXN0LnRlbGVub3Iubm8iLCJhdWQiOiJkZW1vLWNsaWVudCIsImlhdCI6MTY1NjU4NjU2OSwiYXRfaGFzaCI6InRPUFI0ZVhSX1ZCQTI0TS1MY3hvX0EiLCJzX2hhc2giOiJWUWNYMDlvM1RGN05SeXU3OWhwcWFnIiwic2lkIjoiM0YwQUUxRkUzMTUyOUY2ODQyNDg5RDBBQkFERjVENTgiLCJzdWIiOiIyYjQyNDAxMy05NzFiLTQ0MzUtYmRjMS1kMTA3NWIwNWQwZTkiLCJhdXRoX3RpbWUiOjE2NTY1ODY1NjgsImlkcCI6Im5vLnRlbGVub3IuaWQucHJveHkuZGVtby1jbGllbnQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiI0Nzk5OTY2NjM0IiwiZ2l2ZW5fbmFtZSI6IlRvbSBQZXRlciIsImZhbWlseV9uYW1lIjoiUmFzbXVzc2VuIiwiYW5hbHl0aWNzX3V1aWQiOiI3NTY4ZmU1MC0yMTdmLTRhYjgtYTRlNC0yOWNiZTdlMTE4YzkiLCJpYWwiOiJ0ZWxlbm9yLmlkZW50aXR5LmlhbDIiLCJhbXIiOlsidXJuOnRuaWRwbHVzOnN0ZCJdfQ.FNp8QeHGsfGc3EG_3373y8aRgbDJ7TMns9jvndKZ2fFPcP9xZrWuhMmTaf-cY7-dhrgOuvKq80Ud6QbCcFtR6hi_RAH7wTJjaBQznG-l5mBsq6M1t9kKohhPdSkB3tCZFB1sEQT7gcjMn1ubnYwDjHrhjNoK6T8O6pajSf83joC6W5K9NApMCIsDq7ejzC0UVxtqEopZbgeuZkCMg0k5N-G-wRS2bcx_iAl0FfLf7uUChzK2yjW5x_cyYRQcCQATYYIttAiXZOQ9PQNzzwblbHHG2movfFlwN00IjaamBFZLabN1KBchvgh651IPhsa_OB1ZBHW0H4Mu8EXaYPmHCA
```

In this example the ID-token contains the following:


**header**


```
{
  "alg": "RS256",
  "kid": "cffb352e-2a50-4bdb-a553-982fd53b40b1",
  "typ": "JWT"
}
```

**body**

```
{
  "nbf": 1656586569,
  "exp": 1656586869,
  "iss": "https://id-test.telenor.no",
  "aud": "demo-client",
  "iat": 1656586569,
  "at_hash": "tOPR4eXR_VBA24M-Lcxo_A",
  "s_hash": "VQcX09o3TF7NRyu79hpqag",
  "sid": "3F0AE1FE31529F6842489D0ABADF5D58",
  "sub": "2b424013-971b-4435-bdc1-d1075b05d0e9",
  "auth_time": 1656586568,
  "idp": "no.telenor.id.proxy.demo-client",
  "preferred_username": "4799966634",
  "given_name": "Tom Peter",
  "family_name": "Rasmussen",
  "analytics_uuid": "7568fe50-217f-4ab8-a4e4-29cbe7e118c9",
  "ial": "telenor.identity.ial2",
  "amr": [
    "urn:telenor.identity.amr.td_otp_pwd",
    "urn:telenor.identity.amr.td_ok",
    "urn:telenor.identity.amr.bankid"
  ]
}
```

