## Authentication providers

TelenorID\+ supports the following authentication providers:

| Authentication provider | Description  | [AAL](TelenorID_Plus_-_assurance_level.md#authentication-assurance-aal) | [ACR value](TelenorID_Plus_-_idtokens.md#id-token-body) |
|:-----:|:--------------:|:--------:|
| Telenor ID | Default login for Telenor Customers. | 2 | |
| BankID | Login provided by Norwegian banks to Norwegian citizens with a SSN | 3 | ```idp:no.bankid.login.vipps``` |
| Azure | Microsoft Azure AD avaliable for business to business services | 1 | ```idp:no.azure.login``` | 
| TelenorClientCerts | Telenor client certificates | 1 | ```idp:no.telenor.cassowary``` |
| Kookaburra | Kookaburra is a non-personal IDP. It is used by "service phones" (tjenestetelefon), including fixed telephones, to authenticate | 1 | ```idp:no.telenor.kookaburra``` |
| Kiwi | Kiwi is the login for T-we setupboxes without user interaction, giving a login for the T-we setupbox connected to the owner of the setupb box | 1 | ```idp:no.telenor.kiwi``` |

Each authentication providers can have several authentication methods, more information about those: [See more information](TelenorID_Plus_-_idtokens_.md#authentication-methods-referencesamr)


Deprecated:
- Use  value ```acr_values``` =  ```urn:tnidplus:kyc``` to start BankID login, see more information [here](TelenorID_Plus_-_kyc_bankid_-_integration_example_step_by_step.md). 
