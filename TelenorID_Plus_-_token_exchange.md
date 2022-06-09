# TelenorID\+ Token Exchange

TelenorID\+ roadmap is to support the [OAuth 2.0 Token Exchange](https://datatracker.ietf.org/doc/html/rfc8693) with both support for Delegation and Impersonation.

But at the moment TelenorID\+ has only a custom legacy API that is supported. Note that this is deprecated and will be removed when the support for [OAuth 2.0 Token Exchange](https://datatracker.ietf.org/doc/html/rfc8693) is implemented.
This functionality enables clients to change the tnuid in the [access token](TelenorID_Plus_-_accesstokens.md), pointing this to another profile for the same enduser.
This is only relevant for endusers with several profiles.


## Prerequisite

To use the token exchange solution your client needs to be configured to be allowed to use it.
 - The client setting ```Update AT claims``` must be set to ```TRUE``` 
 - The client must be allowed to use the scope ```account.profiles```, more information about scopes [here](TelenorID_Plus_-_scopes.md).

Please [contact us](TelenorID_Plus_-_help.md) to make sure that this is configured.

## Input

The token exchange request is done using the [Token endpoint](TelenorID_Plus_-_token.md) where the following parameters must be set as followed:

| Parameter | Description | 
| ------------- |:-------------:|
| ```scope```	| ```account.profiles``` must be one of the scopes | 
| ```grant_type``` | ```refresh_token```| 
| ```tnuId``` | The tnuid of the profile that you would like to change to |

## Example

```
POST /connect/token
CONTENT-TYPE application/x-www-form-urlencoded

    client_id=client1&
    client_secret=secret&
    grant_type=refresh_token&
    refresh_token=88E0E30...&
    tnuId='nss-hTv...'

```