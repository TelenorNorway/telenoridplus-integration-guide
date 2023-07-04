# How-to setup Azure AD integration towards TelenorID\+ - staging

Step by step instructions for personal at customers who wants to setup a integration from their Azure AD towards Telenor ID\+.
If you have any questions or trouble with this guide please contact us: [our contact details](TelenorID_Plus_-_help.md).

__Note__

> This is the guide for setup in staging, it can be that you setup these steps twice; once for STAGING (this guide) and once for PRODUCTION, instructions found [here](TelenorID_Plus_-_ad_integration_prod.md). 
> It depends on your setup, if you have separate Active Directories.


# Step-by-step instruction


## Phase 1 - Find your Azure AD Tenant ID
1. Find your Azure AD Tenant ID / directory ID
   1. go to portal.azure.com
   2. Login with your local business account
   3. find Azure Active Directory
   4. Copy the TenantID / DirectoryID and use in the next step
2. Send this identifier to Telenor as part of the registration
   1. More info....
wait for confirmation that the Azure AD setup is done and that your azure integration is setup

## Phase 2 - Approve new Telenor Enterprise Application in your tenant
The steps in this phase is all dependent on your Azure tenant configuration.
It all depends on your setting for "consent and permissions" for "enterprise application" in your "Azure AD tenant"
It is three options:
1. End-users can approve the new Telenor enterprise application in your tenant
   - The first end-user logging in to Telenor can approve the new Telenor Enterprise Application for all users in the tenant
2. End-user can apply to approve this application
   - The end-users are stopped and can not login before a local administrator has approved the new Telenor Enterprise Application
   - End-users can apply for approval and a notification is sent to an tenant administrator in your local Azure
   - The administrator in your local tenant needs to approve this new Telenor enterprise application for us in your tenant
3. End-user can not approve or apply for approval of this application
   - The end-users are stopped and can not login before a local administrator has approved this application
   - The end-user must manualy contact local administrator that needs to approve the new Telenor Enterprise Application
   - The local administrator must do the first login towards the Telenor business application with a privileged account and approve the new Telenor enterprise application
