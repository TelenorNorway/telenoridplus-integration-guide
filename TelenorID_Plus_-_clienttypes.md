# TelenorID\+ Client types



## Get Started 

ClientIDs and Secrets are used to authenticate the client with TelenorID\+ when obtaining tokens etc.
These credentials will be different for the different environments (test / prod), and type of client.

For example, if you have three applications, one web, one native Android and one native iOS, you will need to request three TelenorID\+ clients, one for each application. 


### New TelenorID\+ Client with API Access - API Client

If your service needs access to any of the Telenor APIs on APIgee, you will need to request a TelenorID\+ client with API access.

### New TelenorID\+ Client - IBIS Only (no API Access)

If your service only require plain login (and do not need API access on Apigee), you can request a TelenorID\+ client without API access, a IBIS client.


## Client types

In TelenorID\+ we have defined the following client types.


| ID\+ Client Type  | [oAuth Client type](OIDC_basics.md) | [Refresh policy](TelenorID_Plus_-_token_refresh.md) | Description                                    |
|-------------------|-------------------------------------|-----------------------------------------------------|------------------------------------------------|
| Undefined         | N/A                                 | N/A                                                 | not in use                                     |
| Public            | Public                              | NoRefresh                                           | Used by single page application                |
| PublicWithRefresh | Public                              | PublicWeb                                           | Used by single page application                |
| Confidential      | Confidential                        | Confidential                                        | Used by web applications with a secure backend |
| MobileApp         | Public                              | Mobile                                              | Used by a native applications                  |



