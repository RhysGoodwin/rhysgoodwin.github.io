---
id: 518
title: 'ISA Server 2006 Slow Login with LDAP Authentication'
date: '2010-05-19T17:18:56+13:00'
author: RhysGoodwin
excerpt: 'Fix for slow FBA login with Microsoft ISA when using LDAP authentication.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=518'
permalink: /windows-admin/isa-server-2006-slow-login-with-ldap-authentication/
embed:
    - ''
categories:
    - 'Windows Admin'
tags:
    - Authentication
    - FBA
    - 'forms based authentication'
    - ISA
    - 'ISA server 2006'
    - ldap
    - ldaps
    - 'login delay'
    - 'password management'
    - 'slow login'
    - SSL
    - timeout
    - TLS
---

This one had dogged me for ages. There are a number of possible solutions out there but none helped my situation. Today I finally cracked it!

Here’s the situation:

- ISA 2006 standard edition in a perimeter network
- Published HTTPS apps (MOSS 2007 + other misc ASP.NET apps)
- Weblistener with forms based authentication configured to use LDAP with secure connection and without GC
- Internal CA used to provision certs to domain controllers
- Password management features enabled (Hence the requirement for secure connection and no GC)

When users login they get a delay of up to a minute before the authentication screen disappears and their application begins to load. I did some traffic dumps and found that this delay was occurring during the TLS handshake between the ISA server and the LDAP server (domain controller).

Immediately after the LDAP server issues the “Server Hello” there would be a delay, or what appeared to be some kind of a time-out, of about 15 seconds. This occurred several times throughout the authentication process which resulted in the long delay. I confirmed this by disabling the secure connection to the domain controller. This got rid of the delay but of course this wasn’t an option because without SSL you can’t have password management features enabled.

After much digging I eventually ran process monitor on the ISA server and found that there was a RSA machine key which the firewall service (wspsrv.exe) was trying to access which it didn’t have permission to.

```
<em>C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys</em>
```

[![](/content/uploads/2010/05/ProcessMon.jpg "ProcessMon")](/content/uploads/2010/05/ProcessMon.jpg)

Every time it tried to read the key it introduced a few seconds delay as can be seen in the time column above.

The solution: Give the **NETWORK SERVICE** account read access **on that file** (not the whole folder). Of course the file name will be different for each installation so you’ll need to use ProcessMon to find out which file can’t be read by wspsrv.exe.

So this raises a few questions for me, some easier to answer than others:

- How, when and why are these machine keys are used? <span style="color: #ff0000;">\*update\*</span> These are private keys for certificates installed on the server. They are used to encrypt data which is later decrypted using a public key. They are called “machine keys” because they are for the local “machine” account, as opposed to a user account who’s keys would be under C:\\documents and settings\\*username\\…*
- How and by what are these machine keys created? <span style="color: #ff0000;">\*update\* </span>When you create or import a certificate e.g Public certificate for SSL web publishing.
- Is there a specific key associated with an application? <span style="color: #ff0000;">\*update\*</span> The private key is associated with a certificate. Use[ FindPrivateKey.exe](https://blog.rhysgoodwin.com/windows-admin/findprivatekey-exe-pre-compiled/) to help you match keys to certificates
- Why doesn’t ISA server set the correct permission on the machine key file during installation? <span style="color: #ff0000;">\*update\*</span> Not really an ISA fault – ISA runs as NETWORK SERVICE so if that doesn’t have access neither does ISA. I’m still not sure how the permissions get messed up.
- Why doesn’t ISA throw an error to the event log when it couldn’t read the file?
- Why does everything still work even though it can’t read the file! (albeit with significant delay)?

Any input would be much appreciated!

Thanks for reading.