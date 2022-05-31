
# TelenorID\+ Frequently Asked Questions


  - [What should we do when we get `http 400` `invalid_grant` when trying to refresh a access_token?](#what-should-we-do-when-we-get-http-400-invalid_grant-when-trying-to-refresh-a-access_token)
  - [How do I get line authentication when using Telenor ID\+ from a app?](#how-do-i-get-line-authentication-when-using-telenor-id-from-a-app)
  - [What is a BFF?](#what-is-a-bff)
  - [Our application is used together with other applications. Can we log the customer into our application without page refresh when the customer is already on one of these applications and is logged in?](#our-application-is-used-together-with-other-applications-can-we-log-the-customer-into-our-application-without-page-refresh-when-the-customer-is-already-on-one-of-these-applications-and-is-logged-in)
  - [How does your scopes work together with the Apigee scopes?](#how-does-your-scopes-work-together-with-the-apigee-scopes)
  - [Can you get TelenorID\+ without being a Telenor customer?](#can-you-get-telenorid-without-being-a-telenor-customer)
  - [Is "logout URI" mandatory in the client definition on TelenorID\+ for iOS and Android apps?](#is-logout-uri-mandatory-in-the-client-definition-on-telenorid-for-ios-and-android-apps)

<!-- http://ecotrust-canada.github.io/markdown-toc/ Table of contents generated with markdown-toc --> 


## What should we do when we get `http 400` `invalid_grant` when trying to refresh a access_token?

This is the normal response when the access_token is revoked or is expired.
Then you should assume that the user no longer is logged in.
We recommend that you give the end-user the possibility to login again by using our [/authorize](TelenorID_Plus_-_authorize.md) API.


## How do I get line authentication when using Telenor ID\+ from a app?

If you want line authentication, you must use the SDK provided by Telenor ID\+

##  What is a BFF?

BFF stands for Backend For Frontend and it is pattern that is recommended for SPAs for easiere integration and increased security.

## Our application is used together with other applications. Can we log the customer into our application without page refresh when the customer is already on one of these applications and is logged in?

Yes, it should be possible by adding the parameter "prompt=none" i [/authorize](TelenorID_Plus_-_authorize.md) call. 

Please note that some client libraries will automatically have a "fallback" to normal login, but this can largely be overridden.

See more information here [Check session](TelenorID_Plus_-_authorize.md#check-if-user-has-session).


## How does your [scopes](TelenorID_Plus_-_scopes.md) work together with the Apigee scopes?

Each client must explicitly request the scopes it needs in each [/authorize](TelenorID_Plus_-_authorize.md) call.

At TelenorID\+ we support the scopes described [here](TelenorID_Plus_-_scopes.md) and they are declared at our [.well-known discovery endpoint](TelenorID_Plus_-_discovery.md).  The API scopes (scopes for Apigee) is not defined at our [.well-known discovery endpoint](TelenorID_Plus_-_discovery.md)  as they are located and managed on the Apigee / developer portal. This works by TelenorID\+ taking all scopes starting with "api:" and sending them to Apigee for lookup. In practice, a client is stored at both TelenorID\+ and Apigee, but we make sure to keep it in sync. Existing _**api:<apigee\_service>**_ scopes will not change and for example these will still be valid:  
        **_api:mobile-product-v2_**  
        **_api:mobile-subscription-v1_**

 If you gradually see that there are very many, it is up to the developer / designer of api to create "scope collections", eg "invoice.read, invoice.write, invoice.delete" may quickly have to be collected as "invoice". (see [Scopes](https://prima.corp.telenor.no/confluence/pages/viewpage.action?pageId=61425370#TelenorIDPlusIntegrationGuide-Scopes))  
       

##  Can you get TelenorID\+ without being a Telenor customer?

Yes

##  Is "logout URI" mandatory in the client definition on TelenorID\+ for iOS and Android apps?
  
It will not be a very beautiful end user experience if it is not provided. Then we can only show the user a white page with the text "You are now logged out".


