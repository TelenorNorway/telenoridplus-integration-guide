# Scopes

There are two different types of scopes. "Identity scopes" and "API Scopes".

## Identity Scopes

_**The most commonly used scopes and which claims they return.**_

Each scope returns a set of user attributes which are called claims. The scopes an application should request depend on which user attributes the application needs.  
Claims are returned either directly in the id\_token or can be fetched via the /userinfo endpoint.  

| Scope             | Where         | Description/Returned Claims   |
| ----------------- | ------------- | ----------------------------- |
| openid            |  /token       | To indicate that the application intends to use OIDC to verify the user's identity Returns the **sub** claim which uniquely identifies the end user. Returns an ID Token in addition to the Access Token |   
| ial2              |               | Identity Assurance Level 2    |
| profile           | /userinfo     | name (given\_name, middle\_name, family\_name), birthdate, kurtid |
| email             | /userinfo     | email address                 |
| phone             | /userinfo     | phone number                  |
| tnn.ids           | access\_token | kurtid, tnuid                 |
| offline\_access   |               | to get a refresh token        |

### API Scopes

Each client must explicitly request the scopes they need.

API scopes (scopes for access to Telenor APIs on Apigee) will not be visible on well-known as they are located and managed on the Apigee / Developer portal.  
IBIS will take all the scopes that start with "api:" and send them to Apigee for lookup. 
In practice, a client is stored at both IBIS and Apigee, but we make sure to keep it in sync.

Existing api:<apigee\_service> scopes will not change and will remain the same as used today.

[API-EXP-SCOPES](https://prima.corp.telenor.no/confluence/display/APICOUNCIL/API-EXP-SCOPES)