# TelenorID\+ Switch to New Domain id.telenor.no

  * [Background](#background)
  * [TelenorID\+ vs TelenorID](#telenorid---vs-telenorid)
  * [TelenorID\+ \(IBIS\) URIs](#telenorid-----ibis---uris)
    + [Staging](#staging)
    + [Production](#production)
  * [What to Do - Web Clients](#what-to-do---web-clients)
    + [Staging](#staging-1)
    + [Production](#production-1)
  * [What to Do - Native Clients](#what-to-do---native-clients)
  * [What to Do - BankID](#what-to-do---bankid)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Background
Browsers are increasingly blocking third party cookies, particularly if there is no user interaction. As cookies are the only mechanism for "silent SSO" on the web, and silent SSO is an important feature to enable "one Telenor," we need to make a small but important change to Telenor ID\+. We will move our service to telenor.no instead of telenorid.com. This will enable silent SSO for clients using the telenor.no domain.

We have to change the domain for TelenorID\+. This will not affect the "plain TelenorID" \(old ConnectID\).

|Deadline for the switch to new domain *id.telenor.no* is 01 Jun 2021|
  
## TelenorID\+ vs TelenorID
It is only TelenorID\+ \(IBIS\) that changes URIs, not TelenorID \(at Telenor Digital\).
So when using both old TelenorID\+ URIs and new TelenorID\+ URIs, you will still be redirected to *https://signin.telenorid-staging.com/*

## TelenorID\+ \(IBIS\) URIs
### Staging
Replace old URI with New URI.

|  Endpoint              |   NEW URI   |   OLD URI   |
| ------------           | ------------| ------------|
|                        | https://id-test.telenor.no/.well-known/openid-configuration | https://idp.telenorid-staging.com/.well-known/openid-configuration |
| Authorization endpoint | https://id-test.telenor.no/connect/authorize | https://idp.telenorid-staging.com/connect/authorize |
| Token endpoint         | https://id-test.telenor.no/connect/token     | https://idp.telenorid-staging.com/connect/token |
| Userinfo endpoint      | https://id-test.telenor.no/connect/userinfo  | https://idp.telenorid-staging.com/connect/userinfo |

### Production
Replace old URI with New URI.

|Endpoint                |   NEW URI   |   OLD URI |
| ------------           | ------------| ------------|
|                        | https://id.telenor.no/.well-known/openid-configuration | https://idp.telenorid-staging.com/.well-known/openid-configuration |
| Authorization endpoint | https://id.telenor.no/connect/authorize  | https://idp.telenorid.com/connect/authorize |
| Token endpoint         | https://id.telenor.no/connect/token      | https://idp.telenorid.com/connect/token |
| Userinfo endpoint      | https://id.telenor.no/connect/userinfo\) | https://idp.telenorid.com/connect/userinfo |

## What to Do - Web Clients
### Staging
Staging: Replace all occurrences of **idp.telenorid-staging.com** with **id-test.telenor.no**.

### Production
We have to coordinate this change for everything to work properly, but basically it will be to do the same as for staging: Replace all occurrences of **idp.telenorid.com** with **id.telenor.no**.

## What to Do - Native Clients
For those that use our Android or iOS SDKs, please upgrade to the new version \(version 2.0.0 for Android, master branch for iOS\) of the SDKs to use the new domain.

Or replace all occurrences of **idp.telenorid-staging.com** with **id-test.telenor.no** and all occurrences of **idp.telenorid.com** with **id.telenor.no**.

## What to Do - BankID
The same change as for TelenorID\+ applies for BankID.

You have to change the domains used in URIs \(replace all occurrences of **idp.telenorid-staging.com** with **id-test.telenor.no** and all occurrences of **idp.telenorid.com** with **id.telenor.no**\)
