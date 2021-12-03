## TelenorID\+ Authorize

## /authorize

more info to come

## Check if user has Session

The OIDC protocol supports a _**prompt=none**_ parameter on the /authorize request that allows applications to indicate that TelenorID\+ must not display any user interaction - silent authentication.  
TelenorID\+ will either return the requested response back to the application (access\_token), or return an error if the user is not already authenticated or if some type of consent or prompt is required before proceeding.  
If the user was already logged in, TelenorID\+ will respond exactly as if the user had authenticated manually through the login page.  

Be aware that some client libraries will automatically have a "fallback" to normal login, but this can largely be overridden.

If the user was not logged in via Single Sign-on (SSO) or their SSO session had expired, TelenorID\+ will redirect to the specified redirect\_uri (callback URL) with an error:

*   login\_required - The user was not logged in at TelenorID\+, so silent authentication is not possible. TelenorID\+ requires end user authentication.

If an error are returned, an /authorize request without prompt=none parameter must be sent for the end user to be redirected to the login page.