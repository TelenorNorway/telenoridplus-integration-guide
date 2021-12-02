# Seamless Login / SSO

## Single Sign On

[Single sign-on (SSO)](https://en.wikipedia.org/wiki/Single_sign-on) is an authentication scheme that allows a user to log in with a single ID and password to any of several related, yet independent, software systems. The [OIDC](OIDC_basics.md) api provided by Telenor\+ enables SSO for all Telenor services.

In addition to this TelenorID\+ provides several solutions for a more Seamless Single sign-on process where the user can experience a efficient and seamless switch between Telenor services.
And TelenorID\+ provides Telenor services the possibility to disable the reuse of a establish Single sign-on session if needed.


## TelenorID\+ session cookie

TelenorID\+ stores a [web cookie](https://en.wikipedia.org/wiki/HTTP_cookie) in the end-user browser when the user is authenticated.
This cookie is used to recognize the end-user when a new login request is sent and simplify the login process.

Default web application logins will create a cookie that is temporarly stored in the end-user browser as long as the browser is open.
For login request from Mobile app clients or when the end-user explist requests to be remembered the cookie will be persisted, even if the browser is closed.


## One Telenor 

One Telenor is a strategy that shall ensure that the Telenor Norway channels are perceived as coherent.
In TelenorID\+ a client/RP can choose to be part of “One Telenor” or not. A normal client (outside of One Telenor) will generate an isolated TelenorID+ session and have a unique clientID towards Telenor ID (TD).
For the One Telenor clients all clients will share the same TelenorID+ session and have the same ClientID towards TelenorID (TD). 
This will result in a more seamless login experience between the One Telenor clients then the normal clients.


## Silent SSO


## Check if user has Session

The OIDC protocol supports a _**prompt=none**_ parameter on the /authorize request that allows applications to indicate that TelenorID\+ must not display any user interaction - silent authentication.  
TelenorID\+ will either return the requested response back to the application (access\_token), or return an error if the user is not already authenticated or if some type of consent or prompt is required before proceeding.  
If the user was already logged in, TelenorID\+ will respond exactly as if the user had authenticated manually through the login page.  

Be aware that some client libraries will automatically have a "fallback" to normal login, but this can largely be overridden.

If the user was not logged in via Single Sign-on (SSO) or their SSO session had expired, TelenorID\+ will redirect to the specified redirect\_uri (callback URL) with an error:

*   login\_required - The user was not logged in at TelenorID\+, so silent authentication is not possible. TelenorID\+ requires end user authentication.

If an error are returned, an /authorize request without prompt=none parameter must be sent for the end user to be redirected to the login page.