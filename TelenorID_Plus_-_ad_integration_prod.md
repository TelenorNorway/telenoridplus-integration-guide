# How-to setup Azure AD integration towards TelenorID\+ - production

Step by step instructions for personal at customers who wants to setup a integration from their Azure AD towards Telenor ID\+.
If you have any questions or trouble with this guide please contact us: [our contact details](TelenorID_Plus_-_help.md).

__Note__

> This is the guide for setup in production.


# Step-by-step instruction

1. Find your Azure AD TenantID / directoryID
   1. go to portal.azure.com
   2. Login with your local business account
   3. find Azure Active Directory
   4. Copy the TenantID / DirectoryID and use in the next step
2. Send this identifier to Telenor as part of the registration
   1. More info....
3. wait for confirmation that the Azure AD setup is done and that your azure integration is setup
4. Try your first login towards the Telenor business service in staging
   1. dependent on your Azure AD tenant -> enterprise application -> consent and permissions setting you have three different options on the next step.
5. The consent screen that is shown is one of the following:
   1. End-users can approve the new Telenor enterprise application in your tenant
      1. the first end-user approves this for all users in the tenant
   2. End-user can apply to approve this application
      1. The end-user is stopped and can not continue before a local administrator has approved this application
      2. a notification is sent to an tenant administrator in your local Azure
      3. The administrator in your local tenant needs to approve this new Telenor enterprise application for us in your tenant
   3. End-user can not approv or apply for approval of this application
      1. The end-user is stopped and can not continue before a local administrator has approved this application
      2. The end-user must manualy contact local administrator that needs to go through this login process with a privlieged account and approve the new Telenor enterprise application
