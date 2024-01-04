# TelenorID\+ Integration Guide: Integrate from SDK

We provide SDKs to integrate Android and iOS applications with TelenorID\+.


## Benefits
    1. Line authentication when on cellular network
    2. Easier to use than to do it all from scratch

## Prerequisite

### Access to SDKs on Telenor Norway GitHub for Native Clients

If you have native clients and want to use our SDK for Android and/or iOS,
you need to request access to the Telenor Norway repository (if not already member of TelenorNorway).

Send the github account that require access to **telenorid@telenor.no**, 
or request access on Slack [#dc-telenorid-integration-support](https://thedoozers.slack.com/archives/C01DHF39NDA).



## TelenorID\+ SDK - Android
The Android SDK wraps the TelenorID SDK \(from Telenor Digital\) to be able to use line authentication from Telenor Digital, as well as from TelenorID\+.

For communication with the OpenID Connect provider, IBIS for TelenorID\+, the SDK uses "AppAuth for Android" SDK ([https://github.com/openid/AppAuth-Android](https://github.com/openid/AppAuth-Android))

### Example
The Android SDK provides an example application, called ExampleApplication.

#### Configuration
In ExampleApplication.onCreate() we initialize the SDK, IbisSdk.initialize with the applicationContext, 
a custom AppConfig containing clientId, redirectUri and authorizationScope, and if staging or production should be used.

```
    val appConfig = AppConfig(
        clientId = "tnn-example-android",
        redirectUri = Uri.parse("telenorid-example-android://authcallback/"),
        authorizationScope = "openid profile offline\_access")

    IbisSdk.initialize(applicationContext, appConfig, true // if use stage)
```
The redirectUri also needs to be added to build.gradle

```
manifestPlaceholders = \[
	'appAuthRedirectScheme': 'telenorid-example-android'
\]
```
#### Login
In the LoginActivity we check if the user is authorized with IbisSdk.isAuthorized(). If authorized, we start the tokenActivity instead.

The LoginActivity shows a log in button. When clicking that button a custom tab will be opened with the authorize url.

To make sure the line authentication is done before open up the custom tab, we wrap the intent in a callback, IbisSdk.checkLineAuth.

```
    IbisSdk.checkLineAuth(object : LineAuthCallback {
        override fun done() {

           createAuthRequest("")
           try {
                mAuthIntentLatch.await()
            } catch (ex: InterruptedException) {
                Log.w(TAG, "Interrupted while waiting for auth intent")
            }

            val intent: Intent = mAuthService!!.getAuthorizationRequestIntent(
                mAuthRequest.get(),
                mAuthIntent.get()
            )
            startActivityForResult(intent, RC\_AUTH)
        }
    })
```

#### Tokens
After a successful login, exchange the authorization code for tokens.

See TokenActivity exchangeAuthorizationCode.

When doing backend calls that require an access token, use the method performActionWithFreshTokens on the AuthState object. Obtained by IbisSdk.currentAuthState().performActionWithFreshTokens

### UserInfo
Use the fetchUserInfo method in IbisSdk with a TelenorIdCallback to fetch a userInfo object. The method ensures that valid tokens are being used.

## TelenorID\+ SDK - iOS
    1.  When using the SDK for iOS users should be automatically (line authentication) signed in if on the cellular network, and using an IDP that offers it.
    2.  If the user is on WiFi, the SDK should give the application an option to inform the user to turn off the WiFi to be able to be automatically signed in.
    3.  OTP sent by SMS should be automatically copied/ pasted into the input box for OTP for Telenor Id IDP's
    4.  The SDK should use OpenId/AppAuth-iOS SDK for communication with Telenor Id. ([https://github.com/openid/AppAuth-iOS](https://github.com/openid/AppAuth-iOS)) If another client is used, it must first be approved by the DC-AppSec team.
    5.  The SDK must be easily distributed to all Telenor Id clients.

### Line authentication
To be able to do line authentication the SDK needs to do some requests on the cellular network. 
On iOS there is currently not possible to force a request on the cellular network, if the user is on WiFi. 
But there is a way of detecting if the cellular network is being used or not, which gives a possibility of informing the user to turn off WiFi just for a moment to be able to be signed in automatically.

Line authentication should be done in the background before the user even presses any sign in button. 
When the user presses the sign in button, the SDK must check if the line auth session has expired. 
If it has, the SDK should do another line auth, this time synchronized, and use the updated sessionId in the authorize request.
