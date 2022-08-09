---
id: 1236
title: 'ADFS 2.0 in Forest Trust Environment'
date: '2011-10-11T19:52:42+13:00'
author: RhysGoodwin
excerpt: 'ADFS 2.0 Working in a Cross Forest, Forest Trust, or Multi-Domain Environment.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1236'
permalink: /windows-admin/adfs-2-0-in-a-forest-trust-environment/
categories:
    - 'Windows Admin'
tags:
    - ADFS
    - cloud
    - federation
    - saml
---

I’ve been meaning to get this out there for a while now. I’m not going to go into great detail on ADFS but you can get more background on ADFS and federation in these posts:

[Salesforce SSO with ADFS 2.0 – Everything You Need to Know](https://blog.rhysgoodwin.com/cloud/salesforce-sso-with-adfs-2-0-everything-you-need-to-know/)

[ADFS 2.0 Choose Your Attributes Wisely](https://blog.rhysgoodwin.com/security/adfs-2-0-choose-your-attributes-wisely/)

[SAML WebSSO Federation Made Easy](https://blog.rhysgoodwin.com/security/saml-websso-federation-made-easy/)

My scenario is as follows:

- 2 domains in 2 forests with a one way trust between them.  
    (For this post I’ll refer to these domains PERIMETER and INTERNAL)
- PERIMETER trusts INTERNAL but INTERNAL doesn’t trust PERIMETER
- Both PERIMETER and INTERNAL contain user accounts that need to be authenticated and federated via ADFS
- The ADFS server is joined to the PERIMETER domain
- ADFS and its related IIS services need to run under a service account from the INTERNAL domain

Here are the high level hoops I had to jump through to get this working:

1. On a clean Windows 2008 R2 server, obtain and run the ADFS 2.0 setup file AdfsSetup.exe. Select “Federation Server”, This will install everything you need to make ADFS 2.0 work (including pre-requisites). **Don’t run through the config wizard** – We will do the config from command line later.
2. Create a new service account. e.g. INTERNAL\\Svc.ADFS. Create a new DNS ‘A’ record and point it to the ADFS server. E.g. federate.internal.com. Set a Kerberos SPN for the DNS record against the service account: ```
     setspn -a HOST/federate.internal.com stjohn\Svc.ADFS
    ```
3. Load the certificates MMC for local computer account and install a certificate which can be used for the ADFS web site. In the IIS manager configure a new binding on the default website for SSL with the appropriate FQDN and select the cert you just installed.
4. Make sure the ADFS server has access to all LDAP servers for all domains. Something to consider if you’ve got a few firewalls here and there.
5. Add your service account to the local admins group on the ADFS server and to domain admins group for the domain that the service account belongs to. **Don’t panic this will only be temporary!** This just allows the service account to create the necessary config for ADFS in the Program Data\\ADFS OU. Once created it will have the correct permissions for the service account. I had to do this to get it work, not sure why it’s any different to a normal single forest install.
6. Log on to the ADFS server with the service account. <span style="color: #ff0000;"><span style="color: #ff0000;">Skip this step at your peril!</span></span>
7. Run cmd prompt as admin. cd to: ```
    C:\Program Files\Active Directory Federation Services 2.0\
    ```
8. <div><span style="font-size: small;">Run the following command to configure and new ADFS 2.0 farm  
    </span>
    
    ```
    FSConfig.exe CreateFarm /ServiceAccount "INTERNAL\Svc.ADFS" /ServiceAccountPassword "somebiglongpassword" /AutoCertRolloverEnabled /FederationServiceName "federate.internal.com
    ```
    
    <span style="font-size: small;">  
    </span>
    
    </div>
9. <div>Remove the service account from local admins and domain admins now.</div>
10. <div>That’s it. Load the ADFS console and configure ADFS as you would in any other scenario</div>

<span style="text-decoration: underline;">Notes</span>

- <div><span style="font-size: small;">During the install you might get a yellow warning about not being able to set the SPN. That’s cool we already did it above.</span></div>
- Make sure you can view the federation data for your new server e.g. <span style="font-size: small;">[  ](https://federateqa.stjohn.org.nz/FederationMetadata/2007-06/FederationMetadata.xml)https://federate.internal.com/FederationMetadata/2007-06/FederationMetadata.xml</span>
- <span style="font-size: small;"><span style="font-size: small;"><span style="font-size: small;">If you get a certificate error from your service provider. E.g. This typical error from SalesForce:</span></span></span>*Signature or certificate problems  
    Is the response signed? False  
    The signature in the assertion is not valid  
    Is the correct certificate supplied in the keyinfo? False  
    No valid certificate specified in this response.*[![](/content/uploads/2011/10/SFDCSAMLSigError.png "SFDC SAML Certificate Error")](/content/uploads/2011/10/SFDCSAMLSigError.png)<span style="font-size: small;"><span style="font-size: small;"><span style="font-size: small;">  
    T</span></span></span>ry re-generating your token signing certificate using the following PowerShell commands. **Note:**This will break any existing trust relationships you have with any service providers. You will have to export the new cert and update your service providers with it.

```
Add-PSSnapin Microsoft.Adfs.Powershell
Set-ADFSProperties -AutoCertificateRollover $true
Update-AdfsCertificate -Urgent
```

This might not be the only way to get this working and I haven’t tested it thoroughly – your mileage may vary! But as always I’m keen to hear how you get on and happy to field questions.