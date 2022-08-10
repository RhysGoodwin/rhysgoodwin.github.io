---
id: 350
title: 'MOSS Split Back-to-Back in the Real World'
date: '2010-02-01T16:48:14+13:00'

excerpt: 'My personal real-world experience of configuring MOSS 2007 in a split back-to-back topology using Microsoft ISA server 2006'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=350'
permalink: /windows-admin/moss-split-back-to-back-in-the-real-world-part1/
categories:
    - 'Windows Admin'
tags:
    - 'change password'
    - FBA
    - 'forms based authentication'
    - ISA
    - ldap
    - MOSS
    - 'network topology'
    - perimeter
    - sharepoint
    - 'Split back-to-back'
---

I’ve realised that I’m just not going to have time to complete my MOSS back-to-back series in the way I started out in Part 1 so instead I’m going to combine everything in a single post based around my original documentation and go from there. Please feel free to ask questions, I’ll answer them to the best of my ability and continue to add detail to this post.

I found the Microsoft SharePoint extranet deployment documentation pretty lacking; from what I could find they don’t go much further than: *“here are a few ideas, have fun!”* Well I did have fun! And as always with these things the devil is in the details.

### Objectives

**Primary objective – Making SharePoint available over the internet**

- A single URL for external users and internal corporate staff regardless of where they access SharePoint from
- External member accounts contained in a separate domain to internal corporate staff accounts
- Access secured with publicly trusted SSL certificate
- Full backend access and functionality for corporate staff

**Secondary Objectives**

- Implement single sign-on, forms based authentication and reverse proxy for all web-based applications hosted in the perimeter network

**<span lang="EN-NZ">Guiding Principals </span>**

- <span lang="EN-NZ">Internal users logged on to internal workstations are seamlessly authenticated to the MOSS site</span>
- <span lang="EN-NZ">Internal users accessing the MOSS site from the internet are presented with a forms-based logon screen; they authenticate using the same username and password as they do for internal workstation logon</span>
- <span lang="EN-NZ">The same URL is used whether the MOSS site is accessed from internal network or the internet</span>
- <span lang="EN-NZ">Perimeter (non-staff) users access the MOSS site from the internet using an account from the perimeter domain</span>
- <span lang="EN-NZ">Perimeter users cannot logon on to an internal workstations and cannot access internal resources.</span>
- <span lang="EN-NZ">Both internal and perimeter users access the same MOSS site</span>
- <span lang="EN-NZ">Both internal and perimeter users can reset their password from the forms-based logon page</span>
- <span lang="EN-NZ">Once users have logged on they can navigate to other applications without having to enter their username and password again.</span>
- <span lang="EN-NZ">The perimeter domain trusts the internal domain </span>
- <span lang="EN-NZ">The internal domain does **not** trust the perimeter domain</span>
- <span lang="EN-NZ">MOSS WFE applications in the perimeter can use Kerberos to delegate credentials of **internal** users to other applications on the internal domain, so internal users have access to all the same resources regardless of whether they are access from the internet or from an internal workstation</span>

## **Solution Overview**

The solution is built on a back-to-back firewall topology, it uses forms based pre-authentication and server publishing technologies available in Microsoft’s ISA Server 2006. While the hardware firewall is the first point of entry from the internet the MS ISA firewall in the perimeter network should be considered the “front firewall” as it deals with server publishing and user authentication.

From a MOSS 2007 point of view the Microsoft Split back-to-back topology scenario has been followed; MOSS web front-end servers are located in the perimeter network while the application and database servers reside in the internal network.

A one-way Active Directory trust exists between the perimeter domain and the internal domain. This is a non-transitive trust where the perimeter domain trusts the internal domain but not the inverse. This serves two main purposes; it allows windows authentication and delegation to occur between services in the perimeter domain and the internal domain and at the same time protects sensitive data such as payroll databases on the internal network.

[![Solution overview Diagram](/content/uploads/2010/01/overview-1024x521.png "Solution overview Diagram")](/content/uploads/2010/01/overview.png)

**The existing internal network**  
Forest with 2 domains:

- corpforest.com (empty forest root domain)
- corp.com (internal production domain, contains accounts for all corprate staff)

**The new perimeter network**   
Forest with a single domain:

- perimeter.corp.local (perimeter production domain, contains accounts for all members and external collaborators)
- Authenticates users from both perimeter domain and internal domain via domain trust

## Authentication

### Identity and Single Sign On

Everyone who accesses applications in the perimeter falls in to one of the two following categories; these categories determine which domain the user account is created in.

**Internal Corporate Staff:**  
Users that require access to perimeter applications such as SharePoint but also require access to internal resources/applications such as Citrix, payroll / finance databases, and Exchange Email. These users reside in the current corp.com domain

**Members:**  
Users that require access to perimeter applications such as SharePoint but do not require access to internal resources/applications. These users will reside in the new perimeter domain (perimeter.corp.local). External vendors / collaborators also fall in to this category

The ISA server in the perimeter domain is configured to support single-sign-on of users accessing applications on the corp.com DNS name space. This means that a user who logs in to one application will be able to follow a link to another without having to re-authenticate. E.g. A user logs on once to sharepoint.corp.com and can then hop to outlook.corp.com then to citrix.corp.com all without having to re-enter their username/password.

**Forms Based User Authentication**

Users who access web applications from the internet must first authenticate at the front MS ISA firewall. When a user attempts to access a web application (e.g. SharePoint – https://sharepoint.corp.com) in the perimeter network he will first be presented with a forms based (HTML username/password) authentication page at the ISA server. At this point the user can enter a credential from either the perimeter domain or from the internal domain. Once the user’s identity is validated the ISA server will proxy the application to his browser.

### Authentication Protocols

The diagram below outlines the steps taken to authenticate and or delegate user credentials. Basic authentication is used from the ISA server to the published destination IIS server. This is done because cross forest Kerberos delegation is not supported by ISA server. Even though the IIS server *is* a member of the same domain as the ISA server, the IIS application pool is running under a service account from the internal domain, which is done to facilitate windows authentication from the application to the backend database server on the internal network.

The basic credentials are secured using SSL between the ISA server and the destination server.

Once the basic credentials reach the published IIS server it will use Kerberos to authenticate the user against the appropriate domain. Using Kerberos at this stage means that user’s credentials can be delegated to a backend server e.g. SQL Server Reporting Services. The result is that internal users get exactly same experience outside the network as they do inside.

[![User Authentication](/content/uploads/2010/01/User-FBA.png "User Authentication")](/content/uploads/2010/01/User-FBA.png)

**<span style="color: #ff0000;">Internal User </span>**

1. Username / Password from browser sent in clear text (over https) (FBA)
2. ISA pre-authenticates user with internal DC via using LDAPS
3. ISA delegates basic credentials (over https) to MOSS IIS server
4. MOSS IIS authenticates user using Kerberos to internal DC
5. MOSS IIS server delegates Kerberos authentication and impersonates user

<span style="color: #99cc00;">**Member**</span>

1. Username / Password from browser sent in clear text (over https) (FBA)
2. ISA authenticates user with perimeter DC via LDAPS
3. ISA delegates basic credentials (over https) to MOSS IIS server
4. MOSS IIS authenticates user using Kerberos to perimeter DC
5. Delegation fails due to one-way trust. Internal domain doesn’t trust perimeter domain

## Configuration

### Network Configuration

**Networks**

Internal : 192.168.33.0/24  
Perimeter : 172.16.0.0/16  
DMZ : 10.1.1.0/8  
Public : 258.17.84.128/28

**Network relationships**

As shown in the diagram a route relationship is defined between the perimeter and internal network instead of a NAT relationship, this is required to facilitate the domain trust relationship and Kerberos authentication between the two domains.

[![Network Relationships](/content/uploads/2010/01/NetworkRelationships.png "Network Relationships")](/content/uploads/2010/01/NetworkRelationships.png)

**Routing**

All perimeter servers specify the front firewall (ISA02) as their default route

To maximize routing efficiency a persistent static route from the perimeter network to the internal network via the back firewall (ISA01) is added to all perimeter servers. The following command is run during initial server build.

```
<pre style="padding-left: 30px;"><strong>route add 192.168.0.0 mask 255.255.0.0 172.16.1.1 –p
</strong>
```

**IP Addresses**

HW Firewall Internal IF : 10.1.1.254  
ISA01 External IF : 10.10.1.1  
ISA01 Perimeter IF: 172.16.1.254  
ISA02 Perimeter IF : 172.16.1.1  
ISA02 Internal IF : 192.168.33.254  
WFEVIP1 (SharePoint WFE farm load balanced virtual IP): 172.16.1.50

**Network Address Translation**

Public access from the internet is NAT’d via the hardwre firewall. The diagram below shows the complete network path taken by a user when accessing the a web app from the Internet. Note the web listener listens on multiple IP addresses. Multiple web publishhing rules use this web listener.

[![NAT](/content/uploads/2010/01/NAT-1024x478.png "NAT")](/content/uploads/2010/01/NAT.png)

### Back Firewall

**Firewall Policy**

This following table shows the firewall policy on the back firewall (ISA02) that relates to communication between the internal network and the new perimeter network e.g. MOSS frontend/backend/database access. Kerberos, LDAP etc.

This represents the bare minimum of rules as they relate to this document, you’ll certainly have your own additional rules.

<div class="wp-caption aligncenter" id="attachment_433" style="width: 504px">[![Click for an MS Word Version!](/content/uploads/2010/01/Rules-1024x546.png "Rules")](/content/uploads/2010/01/Rules1.doc)Click for an MS Word Version

</div>### Front Firewall

**Web Publishing Rule Configuration**

A single ISA SharePoint publishing rule is used to publish the load balanced MOSS web front end servers. Configuration can be considered default as per the MS ISA SharePoint publishing rule unless detailed below.

**Destination server (To)** The FQDN of SharePoint (sharepoint.corp.com) is specified; this resolves to 172.16.1.50 which is the virtual IP address of the load balanced cluster of MOSS WFE servers. We choose to forward the original host header since the internal and external names are the same. Requests must appear to come from the original client otherwise load balancing based on IP address won’t work.

### [![Web Publish "To" Tab](/content/uploads/2010/01/ISAto.jpg "Web Publish "To" Tab")](/content/uploads/2010/01/ISAto.jpg)

**Listener**

The listener selected (Perimeter Web Listener) is used by all FBA based applications in the perimeter network; this enables SSO functionality.

[![Webpublish Listener selection](/content/uploads/2010/01/Listener.jpg "Webpublish Listener selection")](/content/uploads/2010/01/Listener.jpg)

**Public Name**  
The public DNS name used to match the publishing rule.

[![Web publish public name](/content/uploads/2010/01/PublicName.jpg "Web publish public name")](/content/uploads/2010/01/PublicName.jpg)

**Bridging**  
Connections to the internal servers are only made via SSL, and on the default port (443).

[![Web publish Bridging](/content/uploads/2010/01/Bridging.jpg "Web publish Bridging")](/content/uploads/2010/01/Bridging.jpg)

**Authentication Delegation**  
Credentials are delegated to the target server using basic authentication. Detailed information on why basic delegation is used can be found in the **Authentication Protocols** section under **Solution Overview**.

[![Web publish authenticaion delegation](/content/uploads/2010/01/AuthDelegation.jpg "Web publish authenticaion delegation")](/content/uploads/2010/01/AuthDelegation.jpg)

### Web Listener / Pre-authentication

This section describes the ISA https web listener and forms based authentication configuration. ISA 2006 web listener defaults should be assumed unless described below.

**Networks**

All IP addresses that the web listener will listen on are selected here. There is a separate address defined for each application that makes use of this web listener. Traffic is NAT’d by the hardware firewall from public IP addresses to these DMZ addresses. e.g. the public IP address resolved by sharepoint.corp.com is 258.17.84.142 and is NAT’d to 10.1.1.142

[![Web listener Networks](/content/uploads/2010/01/Netowrks.jpg "Web listener Networks")](/content/uploads/2010/01/Netowrks.jpg)

**Certificates and SSO**

An individual certificate is assigned to each application’s DMZ IP address. This allows multiple SSL secured applications to use the same web listener which consequently means that single sign on can be used across all these applications.

[![Certificates](/content/uploads/2010/01/Certs.jpg "Certificates")](/content/uploads/2010/01/Certs.jpg)

Single sign on is enabled for applications on the corp.com DNS suffix.

[![Web listener SSO Domain](/content/uploads/2010/01/SSO.jpg "Web listener SSO Domain")](/content/uploads/2010/01/SSO.jpg)

**Connections**

http is redirected to https. The good thing here is that http links that that get emailed around when people are working at the office also work when they access from the internet.

[![Web listener connections](/content/uploads/2010/01/Connections.jpg "Web listener connections")](/content/uploads/2010/01/Connections.jpg)

**Forms**

The path to the customised html logon form is specified here as well as the options to allow users to change their passwords and password expiration warning.

[![Web listener forms](/content/uploads/2010/01/Forms.jpg "Web listener forms")](/content/uploads/2010/01/Forms.jpg)**Authentication**

When users first request an application that is using this web listener they must first be authenticated by the MS ISA server before they are allowed to proceed to the requested application. HTML forms based authentication is used for this initial step. This is where user enters their username and password.

As shown in the screenshot LDAP is used instead of native windows authentication; this is due to an issue with password changes in a cross domain scenario. During testing it was discovered that when using Windows authentication it is not possible for a user to change their password at the HTML logon page if their account is in a different domain to the ISA server. This is problematic especially for seldom used accounts where the password expires or for new accounts where the administrator needs to force the user to change their password at next logon. Microsoft ISA 2006 SP1 introduces a bug which also affects the change password functionality when using LDAP authentication, a non-public patch has been released to fix this bug; for details see the known issues section of this document.

Two LDAP server sets are used, one for the perimeter domain controllers and one for internal domain controllers.

<div>Logon expressions are mapped to the LDAP server sets. How users prefix their username determines which LDAP server set is used to authenticate them. The following config uses “**\***” wild card to allow the concept of a default domain. If the login starts with “corp\” then the internal LDAP set is used other wise it default to the perimeter set. This means that anyone who has an account in the perimeter domain doesn’t need to specify a domain.</div>[![Web listener authentication](/content/uploads/2010/01/Authentication.jpg "Web listener authentication")](/content/uploads/2010/01/Authentication.jpg)

Two LAP servers (domain controllers) are defined for each LDAP server set. For password change functionality to work, a secure connection must be used (LDAPS) and the “Use Global Catalog” option must be turned **off**. A lookup account in the target domain must also be made available for the ISA server. This is a non-privileged account with a very strong, non-expiring, set/forget password which is set during configuration.

A secure SSL LDAP connection requires that port 636 be open to the internal domain controllers on the back firewall, this is noted under the “back firewall” configuration earlier in this document.

A server certificates must also be installed on the domain controllers and must be trusted by the ISA server. This can be acheived by setting up a CA (certiificate Authority) but that’s out of scope for this post.

I will however point out a good tip for testing that the ISA server is correctly trusting the DC and is able to make an LDAPS connection. Use the ldp.exe tool from the Windows Server 2003 resource kit and make a connection on port 636 with SSL enabled. If everything is working correctly then it will connect without error otherwise it will throw some kind of TLS error.

[![Web listener LDAP server sets](/content/uploads/2010/01/LDAPServerSet1.jpg "Web listener LDAP server sets")](/content/uploads/2010/01/LDAPServerSet1.jpg)

### Active Directory Trust Configuration

A one-way domain trust is configured between the corp.com domain and the perimeter.corp.local domain. This is done to allow internal accounts to be used in the perimeter domain which inturn enables windows authentication to be used when accessing backend resources such as SQL databases. This trust means that deploying MOSS web front end servers is as straight forward as adding the web front end role, all IIS configuration is completed by MOSS and remains valid in both the internal and perimeter domains.

[![Trusts(perimeter)](/content/uploads/2010/01/Trustsperimeter.jpg "Trusts(perimeter)")](/content/uploads/2010/01/Trustsperimeter.jpg)

[![Trusts(internal)](/content/uploads/2010/01/Trustsinternal.jpg "Trusts(internal)")](/content/uploads/2010/01/Trustsinternal.jpg)

Now you might be wondering why we don’t use a forest trust. Well the truth is I had issues with using a forest trust and if anyone can shed any light on this I’d be very interested. I’d love to set this back up in the lab but I can’t see my self getting time.

Here’s the situation. On the internal side we have an empty forest root domain of <span style="color: #008000;">corpforest.com</span> inside that forest we have the internal production domain called <span style="color: #008000;">corp.com.</span> On the perimeter we have a single forest/domain called <span style="color: #ff6600;">perimeter.corp.local.</span> We create a one-way forest trust (which is transitive) between corpforest.com and perimeter.corp.local. We then take a member server in the perimeter.corp.local domain install MOSS and join it as a webfront end to the MOSS farm based in the internal network. This will automatically create IIS MOSS web apps and application pools. These application pools are set up identically to the ones on the internal MOSS servers so of course are configured to run under **internal** service accounts.

Everything seems to be working correctly the application pools run fine but when trying to browse to the site IIS writes an error: <span style="color: #ff0000;">“The caller is not the owner of the desired credentials”.</span> Despite many hours of digging through logs, traffic sniffing and bashing my head against the desk I was unable to get IIS to work with a service account from another domain when using the forest trust, *except* if I used an account from the root domain of the trusted forest which is not ideal as all internal MOSS IIS servers would need to be re-configured. I should mention that this is not an issue specific to MOSS, it’s an IIS issue in general. I stood up another IIS/ASP.NET/Visual Studio test box in the perimeter and it had the same problem.

If both your internal and perimeter domains are single forest/domain I expect you might not have any issues and I’d be very interested to hear your results.

### SharePoint

This section only describes MOSS configuration that relates to the perimeter network deployment.

**People Picker**

To enable the “PeoplePicker” in a one-way trust scenario to search both domains (perimeter and internal) the following stsadm commands must be run.

On all servers in the farm:

```
stsadm -o setapppassword -password “(password)”
```

(Where “password” is a strong password shared between the servers.

On all web front end servers:

```
stsadm -o setproperty -url http://sharepoint -pn
"peoplepicker-searchadforests"
-pv "domain:perimeter.corp.local",
PERIMETER\PeoplePickerService, (Password)
```

(Where “Password” is the password for the unprivileged service account “PeoplePickerService” used to perform lookups on the perimeter domain)

**AAM (Alternate Access Mappings)**

**Zones**

The following access mappings / zones are configured

[![AAM Zones](/content/uploads/2010/01/AAM.png "AAM Zones")](/content/uploads/2010/01/AAM.png)

Internet users only access sharepoint over SSL in the default zone via https://sharepoint.corp.com

Internal users access sharepoint over plain http in the intranet zone via http://sharepoint.corp.com. This is the default home page for all internal users as set by group policy. Plain HTTP is used for internal users to ensure optimum performance especially across the WAN where the Certeon accelerators are deployed. The URL http://sharepoint is also valid for internal users.

Some SQL Server Reporting Services (SharePoint Integrated Mode) features only work when accessed in the default zone hence internal users wanting to access these features will need to access sharepoint in the default zone via https://sharepoint.corp.com. See the known issues section for more information.

### Known Issues

**MS ISA Forms Based Password Changes**

**Valid Account Discovery Vulnerability**

A patch in ISA 2006 SP1, as means to fix a security vulnerability broke password change functionality when using LDAP authentication. (http://support.microsoft.com/kb/957859/). This has been fixed by non-public patch KB959357 (http://support.microsoft.com/kb/959357). Unfortunately this patch re-introduces the security vulnerability. This Vulnerability means that when an incorrect password is entered for a valid account and the account is in a password-expired state, a change password form is displayed; while the correct old password must be specified before a new one can be set this could allow an attacker to discover that an account name is valid by brute force. This can be considered a low risk vulnerability which is out weighed by the need to allow users to change their own password. Hopefully Microsoft will fix this issue in the next major service pack. Below is a summery of the vulnerability behaviour.

[![ISA Change Password Vulnerability](/content/uploads/2010/01/ISA-Change-Password-Vulnerability.png "ISA Change Password Vulnerability")](/content/uploads/2010/01/ISA-Change-Password-Vulnerability.png)

**Password length / Complexity Policy**

<div>When a password is changed using the ISA FBA change password tool the password complexity is checked against the domain that the ISA server is a member of. This means that both the internal and perimeter domains must have the same password complexity / length requirements to ensure consistent behaviour for end users. *-I need to confirm this one!*</div><div>**SSRS in SharePoint Default Zone Only**When using SQL Server Reporting Services in SharePoint integrated mode some methods of viewing reports are only supported in the default zone. For example, if you try to open a report from a document library while accessing the sharepoint on http://sharepoint.corp.com (intranet zone) instead of https://sharepoint.corp.com (default zone) you will be presented with the following error.

</div>```
The specified path refers to a SharePoint zone that is not supported.
The default zone path must be used.
```

The report viewer webpart works correctly regardless of what zone it is accessed in.

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 1683px; width: 1px; height: 1px;">| **<span lang="EN-NZ" style="font-size: 10pt;">Step</span>** | **<span lang="EN-NZ" style="font-size: 10pt; color: red;">St John</span><span lang="EN-NZ" style="font-size: 10pt; color: red;"> Internal WAN User </span>** | **<span lang="EN-NZ" style="font-size: 10pt; color: #99cc00;">St John</span><span lang="EN-NZ" style="font-size: 10pt; color: #99cc00;"> Member</span>** |
|---|---|---|
| **<span lang="EN-NZ" style="font-size: 10pt;">1</span>** | <span lang="EN-NZ" style="font-size: 10pt;">Username / Password from browser sent in clear text (over https) (FBA)</span> | <span lang="EN-NZ" style="font-size: 10pt;">Username / Password from browser sent in clear text (over https) (FBA)</span> |
| **<span lang="EN-NZ" style="font-size: 10pt;">2</span>** | <span lang="EN-NZ" style="font-size: 10pt;">ISA pre-authenticates user with St John DC via the domain trust</span> | <span lang="EN-NZ" style="font-size: 10pt;">ISA authenticates user with perimeter DC</span> |
| **<span lang="EN-NZ" style="font-size: 10pt;">3</span>** | <span lang="EN-NZ" style="font-size: 10pt;">ISA delegates basic credentials (over https) to MOSS IIS server</span> | <span lang="EN-NZ" style="font-size: 10pt;">ISA delegates basic credentials (over https) to MOSS IIS server</span> |
| **<span lang="EN-NZ" style="font-size: 10pt;">4</span>** | <span lang="EN-NZ" style="font-size: 10pt;">MOSS IIS authenticates user using Kerberos to St John DC via forest trust</span> | <span lang="EN-NZ" style="font-size: 10pt;">MOSS IIS authenticates user using Kerberos to perimeter DC</span> |
| **<span lang="EN-NZ" style="font-size: 10pt;">5</span>** | <span lang="EN-NZ" style="font-size: 10pt;">MOSS IIS server delegates Kerberos authentication and impersonates user </span> | <span lang="EN-NZ" style="font-size: 10pt;">Delegation fails due to one-way trust. Perimeter trusts STJOHN only.</span> |

</div>