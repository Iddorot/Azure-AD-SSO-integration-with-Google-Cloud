# Azure AD SSO integration with Google Cloud TDD

Author: Ido Rot

## Introduction

### Rationale

Accomplish Azure AD SSO integration with Google Cloud / G Suite Connector by Microsoft, Automatic user provisioning from Azure AD to Google Cloud

### Terminology
GCD.COM - The domin name of the google cloud partner
AAD.COM - The domain name of the Azure AD

## Technical Steps


1. Configure Azure AD single sign-on for Google Cloud / G Suite Connector
   1. Adding Google Cloud / G Suite Connector from the **Enterprise Applications** gallery
   1. Setting up Single Sign-On with SAML in the Google Cloud / G Suite Connector
      1. Basic SAML Configuration for Google Cloud Platform: 

|Identifier (Entity ID)|google.com/a/<GCD.COM>|
| :-: | :- |
|Reply URL (Assertion Consumer Service URL)|<p>https://www.google.com/a/<GCD.COM>/acs</p><p>https://www.google.com/</p>|
|Sign-on URL|https://www.google.com/a/<AAD>.COM/ServiceLogin?continue=https://console.cloud.google.com/|
|Relay State (Optional)||
|Logout URL (Optional)|https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0|

1. Download the SAML Signing Certificate(Base64)
1. Copy the Application Links 

1. From **App Registrations** add the Redirect URIs *https://www.google.com/a/<GCD.COM>/acs* 
1. Change the Application ID URI *to https://www.google.com/a<GCD.COM>*
1. Under Permissions we added the following:

![Alt text](Aspose.Words.37f72522-821f-43a6-a506-81afa41a3789.002.png)


1. Create an Azure group “*RZT\_APP\_G\_Suite\_Trafi*” and assign it to the application
1. Add the test user to the group


1. Configure Google Cloud/G Suite Connector by Microsoft SSO
   1. Add reiz.tech domain as a verified domain
   1. Create a group “*RZT\_APP\_G\_Suite\_Trafi*”
   1. Manage SSO profile assignments and add the new group scope to be with Organization's third-party SSO profile 
   1. Set up authentication with SSO with third party IDP
      1. Paste the Login and Logout URL
      1. Upload the SAML Signing Certificate(Base64)
      1. Change Password URL <https://account.activedirectory.windowsazure.com/changepassword.aspx>
   1. Create a replicated test user with the same info as part of the new group
   1. Test the connection with the user

1. Configure G Suite for automatic user provisioning

\**Provisioning works one way, which means changes in Azure AD are replicated to Google Cloud but not vice versa. Also, provisioning doesn't include passwords*

1. Configure G Suite to support provisioning with Azure AD
1. Create replicated provision Service Account [*svc_gsuite_trafi@reiz.tech*](mailto:svc_gsuite_trafi@reiz.tech) and add the user to the group 
1. Create a new delegated admin role and assign it to the svc account
   1. set the following privileges to enabled:
      1. Organization Units > Read
      1. Users
      1. Groups


1. Configure automatic user provisioning **to** G Suite
   1. Create provision Service Account 
   1. Set up provisioning and change to Automatic
   1. Authorize the Service account
   1. Test the connection and save
   1. Mappings section select Provision Azure Active Directory Groups.
      1. Select the “email” mapping and change the mapping type to “Expression”
      1. For expression enter

` `Join("@", NormalizeDiacritics(StripSpaces([displayName])), "reiz.tech")

This operation starts the initial synchronization cycle of all users and groups defined in Scope in the Settings section. The initial cycle takes longer to perform than subsequent cycles, which occur approximately every 40 minutes as long as the Azure AD provisioning service is running.











References:

1. [Tutorial: Azure AD SSO integration with Google Cloud / G Suite Connector by Microsoft - Microsoft Entra | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/google-apps-tutorial#configure-google-cloudg-suite-connector-by-microsoft-sso)
1. [Federating Google Cloud with Azure Active Directory |  Identity and access management](https://cloud.google.com/architecture/identity/federating-gcp-with-azure-active-directory)
1. [Azure AD user provisioning and single sign-on  |  Identity and access management  |  Google Cloud](https://cloud.google.com/architecture/identity/federating-gcp-with-azure-ad-configuring-provisioning-and-single-sign-on)
1. [Assign SSO profile to organizational units or groups - Google Workspace Admin Help](https://support.google.com/a/answer/10723804?hl=en)
1. [Add a user alias domain or secondary domain - Google Workspace Admin Help](https://support.google.com/a/answer/7502379?hl=en)
1. [Tutorial: Configure G Suite for automatic user provisioning with Azure Active Directory - Microsoft Entra | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/g-suite-provisioning-tutorial)
1. [Tutorial - Customize Azure Active Directory attribute mappings in Application Provisioning - Microsoft Entra | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/app-provisioning/customize-application-attributes)




