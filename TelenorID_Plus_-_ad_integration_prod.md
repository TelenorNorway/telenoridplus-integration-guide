# How-to setup Azure AD integration towards TelenorID\+ - production

Step by step instructions for technical personal at customers who wants to setup a integration from their Azure AD towards Telenor ID\+.
If you have any questions or trouble with this guide please contact us: [our contact details](TelenorID_Plus_-_help.md).

__Note__

> This is the guide for setup in production. See separate guide [here](TelenorID_Plus_-_ad_integration_staging.md) for test setup.

## Before you begin

Open a blank text file in your favourite editor. You will need to write down certain details and send them to us. All instructions that requires this are marked with <img src="images/ad/note.png" width="20" height="20">.

# Step-by-step instruction

1. Create a blank text file and start adding company name and contact information
    * <img src="images/ad/note.png" width="20" height="20"> Company: *My company name*
    * <img src="images/ad/note.png" width="20" height="20"> Contact information: *e-mail*  (this should not be a personal email, but a email that can be used also when we need to get i contact several years later) 
1. Log in to the Azure Portal
1. Find Azure Active Directory,
    * then App Registrations and
    * finally New registration
1. New registration;
    * Name: Enter a name for the application. E.g. *telenorid-production-oidc*
    * Supported account types: This depends on your set up and which users you will allow to login with Telenor ID.
      * If you're not sure, select *single tenant*.
    * Redirect URI
      * For production this is: https://id.telenor.no/signin-{company-name}
      * Note: Company name can only contain lower case letters and hyphens. E.g: https://id.telenor.no/signin-real-power-tools-inc
      * <img src="images/ad/note.png" width="20" height="20"> write down the URI you've chosen
1. After completing the initial registration you should be redirected to the newly created client's Overview page.
    * <img src="images/ad/note.png" width="20" height="20"> Copy "*Application (client) ID*" and paste them in the text file.
    * <img src="images/ad/note.png" width="20" height="20"> Copy "*Directory (tenant) ID*"and paste them in the text file.
1. Go to "Certificates & secrets", Click "New client secret".
    * Fill in an appropriate description
    * Set:Expires to *24 months*.
    * <img src="images/ad/note.png" width="20" height="20"> Copy the client secret and put it in the same file as your client- and tenant ids. Prefix it with "Secret:"
    * <img src="images/ad/note.png" width="20" height="20"> Add the expiration date to the text file
1. <img src="images/ad/note.png" width="20" height="20"> Save the text file as *telenorid-production-{company-name}.txt*.
    * The text file should be something like the example below
8. Exchange the file with the client information and client secret
   * [See details on how to exchange sensitive information with us](TelenorID_Plus_sensitive_data_exchange.md)
9. Set up a reminder in approx 23 months reminding you to create a new client secret
    * Take contact with us before the client secret expires to coordinate the renewal process


## Example file
The content of your text file (*telenorid-production-{company-name}.txt*) should be something like this:

```
  Company: My company name
  Contact information: persistantemail@company.no
  Redirect url: https://id.telenor.no/signin-real-power-tools-inc
  Application (client) ID:  3ffebade-dd8f-460d-bee9-b82e8a4edae7
  Directory (tenant) ID: 3a238dd1-86d1-49ce-9beb-3f8453b0cb21
  Client secret: Secret:b82e8ab82e8a4edae74edae7
  Client secret expiration date: 2025-02-12 12:23:23
```
