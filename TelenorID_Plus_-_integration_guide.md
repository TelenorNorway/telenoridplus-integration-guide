# TelenorID\+ Integration Guide

 


## Get Started With TelenorID\+

**TelenorID\+ offers login through the OpenID Connect (OIDC) protocol. 
This article contains only a basic introduction to the protocol. 
OIDC is a well established industry standard for authentication and authorisation and so material are available;
check the [related articles](#related-articles).**

Functionally the TelenorID\+ solution is almost identical to FellesID.
TelenorID\+ uses the OIDC protocol in addition to OAuth (FellesID).

OIDC is a superset of OAuth so it has some extra features.
Primarly that it is statefull and can keep track of a user's session. 
OIDC also issues an id\_token containing basic information about the user which also can be 
used to verify that a user is logged in.

ClientIDs and Secrets (for Confidential clients) are used to authenticate the client with TelenorID\+
when obtaining tokens etc.
These credentials will be different for the different environments (test / prod), and type of client.

It is very important to use the correct client type for your application due to different security considerations. 
[Single Sign On (SSO)](#seamless-login--sso) and token refresh also differs between the native and web clients.

For example, if you have three applications, one web, one native Android and one native iOS,
you will need to request three TelenorID\+ clients, one for each application. 

### New TelenorID\+ Client with API Access - API Client

If your service needs access to any of the Telenor APIs on APIgee, you will need to request a TelenorID\+ client with API access.

*   If your service is new to TelenorID\+, check out [TelenorID\+ Onboarding of New Services](#TelenorID\+OnboardingNewClients-TelenorID\+OnboardingofNewServices).
*   If you are already familiar with the Developer Portal, and your service is already using TelenorID\+, check out [Additional TelenorID\+ Client with API Access - API Client](#TelenorID\+OnboardingNewClients-AdditionalTelenorID\+ClientwithAPIAccess-APIClient).

### New TelenorID\+ Client - IBIS Only (no API Access)

If your service only require plain login (and do not need API access on Apigee), you can request a TelenorID\+ client without API access, a IBIS client.

*   f your service is new to TelenorID\+, check out [TelenorID\+ Onboarding of New Services](#TelenorID\+OnboardingNewClients-TelenorID\+OnboardingofNewServices).
*   If you are already familiar with the Developer Portal, and your service is already using TelenorID\+, check out [Additional TelenorID\+ Client - IBIS Only (no API Access)](#TelenorID\+OnboardingNewClients-AdditionalTelenorID\+Client-IBISOnly(noAPIAccess)).

### Client Type - Confidential vs Public Clients

| Client Type   | Description   | Pros & Cons |
| ------------- | ------------- | ------------|
| Confidential  | Any client that will run in a secure environment and will store the client credentials in a secure way; | Benefits - can typically access all scopes. |
|               | typically only servers. Examples are traditional back-ends or back-end for front-ends (BFFs) | Can request refresh\_tokens ("offline access") |
| Public        | Traditional SPAs, Native (desktop or mobile) apps | Drawbacks: Can not get a refresh\_token |
|               |              | Session refresh must be done through "silent refresh" |

### Scopes

There are two different types of scopes. "Identity scopes" and "API Scopes".

#### Identity Scopes

_**The most commonly used scopes and which claims they return.**_

Each scope returns a set of user attributes which are called claims. The scopes an application should request depend on which user attributes the application needs.  
Claims are returned either directly in the id\_token or can be fetched via the /userinfo endpoint.  

| Scope             | Where         | Description/Returned Claims   |
| ----------------- | ------------- | ----------------------------- |
| openid            |  /token       | To indicate that the application intends to use OIDC to verify the user's identity Returns the **sub** claim which uniquely identifies the end user. Returns an ID Token in addition to the Access Token |   
| ial2              |               | Identity Assurance Level 2    |
| profile           | /userinfo     | name (given\_name, middle\_name, family\_name), birthdate, kurtid |
| email             | /userinfo     | email address                 |
| phone             | /userinfo     | phone number                  |
| tnn.ids           | access\_token | kurtid, tnuid                 |
| offline\_access   |               | to get a refresh token        |

#### API Scopes

Each client must explicitly request the scopes they need.

API scopes (scopes for access to Telenor APIs on Apigee) will not be visible on well-known 
as they are located and managed on the Apigee / Developer portal.  
IBIS will take all the scopes that start with "api:" and send them to Apigee for lookup. 
In practice, a client is stored at both IBIS and Apigee, but we make sure to keep it in sync.

Existing api:<apigee\_service> scopes will not change and will remain the same as used today.

[API-EXP-SCOPES](https://prima.corp.telenor.no/confluence/display/DC/API-EXP-SCOPES)

### Language

Use **_ui\_locales=no_** to choose Norwegian language.

### Access to SDKs on Telenor Norway GitHub for Native Clients

If you have native clients and want to use our SDK for Android and/or iOS,
you need to request access to the Telenor Norway repository (if not already member of TelenorNorway).

Send the github account that require access to **dc\_security@telenor.com**, 
or request access on Slack [#dc-telenorid-integration-support](https://thedoozers.slack.com/archives/C01DHF39NDA).

### IBIS URIs in the Staging and Production Environments

We URIs have been changed/are about to change, see [TelenorID\+ Switch to new domain id.telenor.no](TelenorID_Plus_-_switch_to_new_domain.md) for more information.

| Endpoint                  | STAGING                                       | PRODUCTION                                |
| ------------------------- | --------------------------------------------- | ----------------------------------------- |
|                           | https://id-test.telenor.no/.well-known/openid-configuration | https://id.telenor.no/.well-known/openid-configuration |
| Authorization endpoint    | https://id-test.telenor.no/connect/authorize  | https://id.telenor.no/connect/authorize   |
| Token endpoint            | https://id-test.telenor.no/connect/token      | https://id.telenor.no/connect/token       |
| Userinfo endpoint         | https://id-test.telenor.no/connect/userinfo   | https://id.telenor.no/connect/userinfo    |
| Check session endpoint    | https://id-test.telenor.no/connect/checksession | https://id.telenor.no/connect/checksession |
| Logout endpoint           | https://id-test.telenor.no/connect/endsession | https://id.telenor.no/connect/endsession  |

### Debugger

The debugger is a useful tool to validate that your client is correctly configured and has access to the scopes you accept.  
All public clients (i.e. "PKCE clients") are configured to work with the debugger.

[https://oidc-test.telenor.no/](https://oidc-test.telenor.no/)





