## TelenorID\+ Q & A

1.  Does it mean that you also implement support for line authentication when using Telenor ID+ for apps, if you follow the advice on "We strongly encourage the use of our SDKs for integration"?
    1.  Yes. If you want line authentication, you must use the SDK.
2.  We currently have a Web app (MBN) with a backend (Talos) whose only task is to handle the client secret for the app. Is it then recommended when switching to Telenor ID to switch to "authorization code with PKCE" and skip the backend?
    1.  We recommend keeping the backend and using "[Authorization Code Flow With ClientID and Secret](https://prima.corp.telenor.no/confluence/pages/viewpage.action?pageId=61425370#TelenorIDPlusIntegrationGuide-TelenorIDPlusOIDCAuthorizationCodeFlowWithClientIDandSecret)" and a confidential web client.
3.  The Telenor Chat application currently runs i.a. in my telenor and on [telenor.no](http://telenor.no). Can we log the customer into the chat client without page refresh when the customer is already on one of these services and is logged in?
    1.  Yes, it should be possible by adding the parameter "prompt=none" i /authorize call.  
        Please note that some client libraries will automatically have a "fallback" to normal login, but this can largely be overridden (see [Seamless Login / SSO](#seamless-login--sso))
4.  At [https://id-test.telenor.no/.well-known/openid-configuration](https://id-test.telenor.no/.well-known/openid-configuration) the following scopes are supported: "scopes\_supported":\["ial3","ial2","ial1","profile","openid","email","phone","tnn.legacy","saml","tnn.ids","tnn.profile.attributes","offline\_access"\]  
    
    How will this be regarding scopes against Apigee? Will one scope per client be created (eg tnn.smartansatt) that has all the scopes this client has access to? Or will each auth flow have to ask for scopes for the different services to be used?  
    1.  Each client must explicitly request the scopes it needs in each /authorize call. If you gradually see that there are very many, it is up to the developer / designer of api to create "scope collections", eg "invoice.read, invoice.write, invoice.delete" may quickly have to be collected as "invoice". (see [Scopes](https://prima.corp.telenor.no/confluence/pages/viewpage.action?pageId=61425370#TelenorIDPlusIntegrationGuide-Scopes))  
        API scopes (scopes for Apigee) will not be visible on IBIS well-known as they are located and managed on the Apigee / developer portal. This works by IBIS taking all scopes starting with "api:" and sending them to Apigee for lookup. In practice, a client is stored at both IBIS and Apigee, but we make sure to keep it in sync. Existing _**api:<apigee\_service>**_ scopes will not change and for example these will still be valid:  
        **_api:mobile-product-v2_**  
        **_api:mobile-subscription-v1_**
5.  The "integration guide" states the following for public clients: _Drawbacks: Can not get a refresh\_token_ _Session refresh must be done through "silent refresh".  
    _Does "silent refresh" mean that you should use /authorize with "prompt = none"?
    1.  Correct, but typically you do it in an iframe that is transparent to the user (hence the name, "silent refresh")
6.  Can you get TelenorID\+ without being a Telenor customer?
    1.  Yes
7.  Is "logout URI" mandatory in the client definition on IBIS for iOS and Android apps? (It says in the form that it is "mandatory" regardless of client type)
    1.  It will not be a very beautiful end user experience if it is not provided. Then we can only show the user a white page with the text "You are now logged out".
8.  _to be continued..._