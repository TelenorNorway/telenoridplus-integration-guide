# TelenorID\+ assurance levels

TelenorID\+ uses different terms when it comes to identity and authentication assurance levels.
We recognize both the [NIST SP 800-63-3](https://pages.nist.gov/800-63-3/) and the [eIDAS Regulation (EU)](http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv%3AOJ.L_.2014.257.01.0073.01.ENG) that uses some different terms.
The reader should be familiar with these standards.


## Identity assurance levels (IAL)
In TelenorID\+ we recognize three distinct identity assurance levels (IAL) as defined in the standard [NIST SP 800-63-3 Digital Identity Guidelines](https://pages.nist.gov/800-63-3/):

| Level |                                                                                                         Description                                                                                                         |              Examples               |
|:-----:|:-------------------------------:|:-----------------------------------:|
| IAL0  | A shared account not connected to an identity.                                                                     | A shared service phone used by several persons, a shared T-We set top box  |
| IAL1  | A self asserted identity with no effort made to ensure that the identity is real.                                                                      | Telenor ID, Facebook, Apple, Google |
| IAL2  |  It has been established that the asserted identity is in fact a real identity, and some effort has been made to ensure that the identity belongs to the entity claiming it.                         |            Telenor ID\+.            |
| IAL3  | It has been established that the asserted identity is in fact a real identity. Documentation proving the relationship between the identity and the entity claiming it has been provided and verified by a competent entity. |              BankID.               |


The IALs can be required as scopes from TelenorID\+ if your service needs to know. In principle an IAL0 also exists for so-called "non personal users" or "service phones." This is an IAL suitable to perform actions anyone with access to the handset should be able to, regardless of who that person is, and that handset is not dedicated to any specific user.

By having the right IAL for a given service, we can make first-time sign-up as easy or hard as it needs to be. Signing up to an IAL1-service would be very simple and non-intrusive, whereas signing up to an IAL3 service will require a lot more effort from the end-user. IAL2 is a compromise for services that require some certainty as to the identity of the end users, such as Mitt Telenor.

The client should request the lowest IAL it can work with. TelenorID\+ will always return the highest IAL in the ID-token and at the /userinfo endpoint in the ial-claim. Based on this one can make authorization decisions as to what features or actions should be available to the user.

## Authentication assurance (AAL)

Like identity assurance, TelenorID\+ recognizes three distinct levels of authentication assurance (AAL) as defined in the standard [NIST SP 800-63-3 Digital Identity Guidelines](https://pages.nist.gov/800-63-3/):


| Level |   Description   | [Authentication providers](TelenorID_Plus_-_authentication_providers.md) | [ACR value](TelenorID_Plus_-_idtokens.md#id-token-body) |
|-------|-----------------|--------|--------|
| AAL1  |  A single authentication factor, such as password or a OneTimePassword               | Telenor ID one-factor, Telenor ClientCerts for Partners, T-we set top box, Azure AD for Business  | `["urn:telenor.identity.aal.1"]` |
| AAL2  | two independent authentication factors, eg. "something you know" like a password, and "something you have" such as an OTP or a password. | Telenor ID  | `["urn:telenor.identity.aal.2"]` |
| AAL3  | an authenticator cryptographically linked to the account, such as a Bank ID dongle or SIM-card or a FIDO authenticator device.      | BankID         | `["urn:telenor.identity.aal.3"]` |

Note that in [OIDC](OIDC_basics.md) or OAuth nomenclature, an AAL is typically expressed as an [Authentication Context Reference (ACR)](TelenorID_Plus_-_idtokens.md#id-token-body). 

By selecting the right AAL for a given service, we can make logging on as easy or as hard as it needs to be. Logging into an AAL1-service will be very simple and non-intrusive, while logging into an AAL3 service will require some effort from the end-user. AAL2 offers a good compromise between security and convenience for services that do not protect high value assets for the user.

## eIDAS Levels of Assurance (LoA)
Under the [eIDAS Regulation (EU)](http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv%3AOJ.L_.2014.257.01.0073.01.ENG) [910/2014](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=uriserv%3AOJ.L_.2014.257.01.0073.01.ENG), electronic identification (eID) schemes are classified according to three levels of assurance. 

|    Level    |                                                                                          Description                                                                                           |
|:-----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|     Low     |                                           for instance, enrolment is performed by self-registration in a web-page, without any identity verification                                           |
| Substantial | for instance, enrolment is performed by providing and verifying identity information, and authentication by using a user name and a password and a one-time password sent to your mobile phone |
|    High     |                         for instance, enrolment is performed by registering in person in an office, and authentication by using a smartcard, like a National ID Card.                          |

This law is regulated through the Norwegian law [lov om elektroniske tillitstjenester LOV-2018-06-15-44](https://lovdata.no/dokument/LTI/lov/2018-06-15-44) and is assumed required for Telenor in a updated ekom law in the future: [Høring - Forslag til ny ekomlov](https://www.regjeringen.no/no/dokumenter/horing-forslag-til-ny-ekomlov-ny-ekomforskrift-og-endringer-i-nummerforskriften/id2864853/)
