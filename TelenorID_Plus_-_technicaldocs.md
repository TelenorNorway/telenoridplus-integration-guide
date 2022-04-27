# TelenorID\+ Technical doc

## Technical introduction
* [Client types](TelenorID_Plus_-_clienttypes.md)
* [Scopes](TelenorID_Plus_-_scopes.md)
* [TelenorID\+ Seamless Single Sign On (SSO)](TelenorID_Plus_-_SSO.md)

## [API docs](TelenorID_Plus_-_api.md)

* [Discovery endpoint](TelenorID_Plus_-_discovery.md)
* [Authorize endpoint](TelenorID_Plus_-_authorize.md)
  * [Check session](TelenorID_Plus_-_authorize.md#check-if-user-has-session)
  * [Force authentication](TelenorID_Plus_-_authorize.md#force-authentication-and-ignore-sso)
* [Token endpoint](TelenorID_Plus_-_token.md)
  * [Access Token](TelenorID_Plus_-_accesstokens.md)
  * ID Token
  * [TelenorID\+ Token refresh](TelenorID_Plus_-_token_refresh.md)
* [Userinfo endpoint](TelenorID_Plus_-_userinfo.md)
* [End session endpoint (Logout)](TelenorID_Plus_-_logout.md)
* revocation_endpoint (todo)
* introspection_endpoint (todo)
* device_authorization_endpoint (todo)
* [selfservice endpoint (Manage my Telenor)](TelenorID_Plus_-_ManageMyTelenor.md)

The API doc for Telenor Digital Telenor ID can be found [here](https://docs.telenordigital.com/connect/id).

## Integration help

* [How-to send us sensitive data](TelenorID_Plus_sensitive_data_exchange.md)
* [Info for native applications](TelenorID_Plus_-_NativeClients.md)
* [Integrating Native Apps Using Our SDK](TelenorID_Plus_-_telenorid_from_sdk.md)
* [How-to setup Azure AD integration towards TelenorID - staging](TelenorID_Plus_-_ad_integration_staging.md)
* [How-to setup Azure AD integration towards TelenorID - production](TelenorID_Plus_-_ad_integration_production.md)
* [Need support?](TelenorID_Plus_-_help.md)
  
The debugger is a useful tool to validate that your client is correctly configured and has access to the scopes you accept.  
All public clients (i.e. "PKCE clients") are configured to work with the debugger.
* [https://oidc-test.telenor.no/](https://oidc-test.telenor.no/)  

## Examples

 * [Simple User Login Example using the open TelenorID\+ debugger](TelenorID_Plus_-_user_login_-_integration_example_step_by_step.md)
 * [TelenorID\+ BankId / "Know Your Customer (KYC)" example](TelenorID_Plus_-_kyc_bankid_-_integration_example_step_by_step.md)

## Historic topics

* [2021 - TelenorID\+ switch to new domain](TelenorID_Plus_-_switch_to_new_domain.md)

## Releated Articles

 * [TelenorID api doc](https://docs.telenordigital.com/connect/id)
 * [Related external resources on OIDC](RelatedArticles.md)