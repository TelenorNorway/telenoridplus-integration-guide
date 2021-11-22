## Native Clients (Android/iOS/..)

### TelenorID\+ OIDC Authorization Code Flow With PKCE

![OIDC Authorization Code Flow with PKCE](https://www.websequencediagrams.com/files/render?link=yllAjtTxwQ8X28I7VNazjDm0qU0e1BTYSrAecMtS8PkQA2huVqJDaoFmg5iGhaP4)

[Standard OIDC Authorization Code Flow (PKCE)](TelenorID_Plus_-_standard_oidc_flows.md#standard-oidc-authorization-code-flow-pkce) for reference.

|                              | 
| ---------------------------- |
| We **strongly encourage** the use of our SDKs for the integration. |
| This will simplify the integration, help avoid common pitfalls/bugs, and ease upgrading to newer versions of the protocol. | 
| When using our SDKs you will also automatically get **line authentication** when on a cellular network. |
| More information on our SDKs for both Android and iOS can be found here: [TelenorID\+ integration using SDK](TelenorID_Plus_-_telenorid_from_sdk.md) |


*   A native app must not wrap a login flow implemented in a web application. This violates security requirements since it offers login in an unidentified window (without displaying the domain of the authorization server in a browser).  
    The proof of authorization, which for OAuth is the auhorization code, should be delivered directly to the app code, not via some web application. Use Android App Links or Apple Universal Links to do this in the most secure way.
*   To increase the likelihood of getting SSO, the default browser must be used, rather than forcing usage of a specific browser like Chrome. Also, you should use custom tabs for the authentication flow to get a smoother integration.
*   A client is must maintain a session towards the authorization server using tokens and must notice when it has lost authorization (the tokens have become invalid). This can easily be tested by removing access for the client in the Telenor Digital end user self-service solution at [https://connect.telenordigital.com/gui/mypage/manageclientauthorizations](https://connect.telenordigital.com/gui/mypage/manageclientauthorizations), and checking whether the client is considering the end user logged out.

### Authentication Using Client ID and PKCE

Before the /authorization request, the client app will generate the **code\_verifier**, a cryptographically random string using the characters A-Z, a-z, 0-9, and the punctuation characters -.\_~ (hyphen, period, underscore, and tilde), between 43 and 128 characters long.

The **code\_verifier** is then used to generate the **code\_challenge**. The code\_challenge is a BASE64 URL encoded string of the SHA256 hash of the **code\_verifier**.

The **code\_challenge** and the **code\_challenge\_method** are sent on the /authorization request along other parameters. The **code\_verifier** is sent on the /token request so the authorization server can verify that the client requesting the tokens are the same that did the /authorization request.

### Android

Read [Handling Android App Links](https://developer.android.com/training/app-links), and especially [Verify Android App Links](https://developer.android.com/training/app-links/verify-site-associations)

**[TelenorID Plus Android SDK](https://github.com/TelenorNorway/telenorid-sdk-android)**

You configure the SDK with your client credentials and for which environment they are configured, either staging or production.

You need to [request access](#access-to-sdks-on-telenor-norway-github-for-native-clients) to the GitHub Repository if not already a member of TelenorNorway.

More information: [TelenorID SDK - Android](TelenorID_Plus_-_telenorid_from_sdk.md#telenorid-sdk---android)

### iOS

Article on [Universal links for Android and iOS](https://medium.com/bumble-tech/universal-links-for-android-and-ios-1ddb1e70cab0)

[Adding support for universal link on iOS](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html) (apple.com)

**[TelenorID Plus iOS SDK](https://github.com/TelenorNorway/telenorid-sdk-ios)**

You configure the SDK with your client credentials and for which environment they are configured, either staging or production.

You need to [request access](#access-to-sdks-on-telenor-norway-github-for-native-clients) to the GitHub Repository if not already a member of TelenorNorway.

More information: [TelenorID SDK - iOS](TelenorID_Plus_-_telenorid_from_sdk.md#telenorid-sdk---ios)