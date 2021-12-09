## Request

To request an access_token, use the /token endpoint.

## The access token

TelenorID\+ issues by_value access tokens that is self-contained, meaning it contains all the relevant information about the authorization (end user, scope, timestamp etc.). Such tokens are non-revokable and should have a short lifetime.

The token is a JWT with the following structure:

### Access tokens header:

| Claim                | Value                                                                                                                                                              |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```kid``` “Key identifier” | unique identifier for the key and certificate used by TelenorID\+. The corresponding public key and the certificate must be fetched from our .well-known endpoint. |
| ```alg``` “algorithm”      | algorithm used for signing the token. TelenorID\+ only supports RS256 (RSA-SHA256)                                                                                 |
| ```typ``` "Token type"     | always: "at+jwt", describing that this is a JSON Web Token                                                                                                         |

### Access token body:

| Claim     | Value                                                                        | Example                                  |
|-----------|------------------------------------------------------------------------------|------------------------------------------|
| ```nbf```       | Not before - identifies the time before which the JWT MUST NOT be accepted.  | 1639039167                               |
| exp       | Expire - Timestamp when this token should not be trusted any more.           | 1639042767                               |
| ```iss```       | The identifier of TelenorID\+ as can be verified on the .well-known endpoint | https://id-test.telenor.no               |
| client_id | The client_id of the client who received this token. Note that client_ids should in general not be used for access control.                                                                                   | tnn-mbn-android-test                     |
| ```sub```       | “subject identifier” - an unique identifier for the authenticated user. The value is pairwise, meaning a given client will always get the same value, whilst different clients do not get equal values for the same user.  |                       2b424013-971b-4435-bdc1-d1075b05d0e9     |
| ```auth_time``` | Timestamp when this token was created.                                       | 1639039166                               |
| ```idp```       |                                                                              | no.telenor.id.proxy.tnn-mbn-android-test |
| ```ibis.sid```  | Internal session id in TelenorID\+                                           | cf2b1147-c2e7-4c74-a32e-3afdb034e181     |
| ```jti```       | jwt id - unique identifer for a given token                                  | 597ED8B9720FE0CBFC844063D7FED863         |
| ```sid```       |                                                                              | 70A6B33AB0D906BC3D203AE946CCC63B         |
| ```iat```       | Timestamp when this token was issued.                                        | 1639039167                               |
| ```scope```     | A list of scopes the access_token is bound to.                               | `["openid","profile","ial2"]`            |
| ```amr```       |                                                                              | `["urn:tnidplus:std"]`                   |

## Access token validation

**The client and resource server MUST validate all responses from TelenorID\+ according to the OIDC and Oauth2 standards as well as practice recommendations from the IETF.**

Access tokens must always be validated by the Resource Server / API before granting access. Clients should normally just pass the access token along to the resource server without any processing of it, however if any processing is performed, clients must also perform such validation.

The following references are vital:

- [RFC6819: Oauth2 threat model](https://tools.ietf.org/html/rfc6819)
- [RFC8252: OAuth 2.0 for Native Apps](https://tools.ietf.org/html/rfc8252)
- [Draft RFC: OAuth 2.0 for Browser-Based Apps](https://tools.ietf.org/html/draft-ietf-oauth-browser-based-apps-03)
- [Draft RFC: OAuth 2.0 Security Best Current Practice](https://tools.ietf.org/html/draft-ietf-oauth-security-topics-13)

Developers integrating towards TelenorID\+ are expected to know these documents and apply them in their risk assessments. Our documention will not re-iterate the recommendations in the above documents, but overall we would like to hightlight:

- **Use a certified / well recognized IAM-product or OIDC library for the integration**
- **Keep secrets safe to avoid getting your service impersonated**
