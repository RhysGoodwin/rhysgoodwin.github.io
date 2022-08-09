---
id: 867
title: 'vbScript &#8211; Add to Local Group with Alternate Credentials'
date: '2010-12-07T18:33:41+13:00'
author: RhysGoodwin
excerpt: "A vbScript snippet to add a domain group to a local group using alternate credentials.\r\n"
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=867'
permalink: /windows-admin/vbscript-add-to-local-group-with-alternate-credentials/
categories:
    - 'Windows Admin'
tags:
    - 'active directory'
    - 'Alternate Credentials'
    - 'domain account'
    - 'domain group'
    - 'local group'
    - password
    - useranme
---

This function adds a domain group to a local group but uses alternate credentials than that of the user account running the script.  
[![](/content/uploads/2010/12/AddtoLocal.jpg "Local Groups")](/content/uploads/2010/12/AddtoLocal.jpg)  
This is handy when you’re doing automated deployments and you’re using auto admin login for post-install configuration.

Here’s the syntax (Note the forward slash to specify the name of the group you want to add the back slash for the credential). The function returns 0 if all is well.

```
<strong>AddToLocalGroup</strong>(LocalGroup, DomainGroup, DomainUser, Password)
<strong>e.g.
AddToLocalGroup(</strong>"Administrators", "domain/groupToAdd","MyCompany\RhysAdmin","secretepassword"<strong>)</strong>
```

And here’s the function

```
<span style="color: #0000ff;">Function </span><span style="color: #000000;">AddToLocalGroup</span>(strLocalGroup,strDomainSlashGroup,strUsername,strPassword)
<span style="color: #0000ff;"> on error resume next </span>
 <span style="color: #0000ff;">Dim </span>strDctrl, strServerName, objDomainGroup, objLocalAdmGroup
 <span style="color: #0000ff;">Const </span>ADS_SECURE_AUTHENTICATION = &H1
 <span style="color: #0000ff;">Set </span>oLocalAdmGroup = GetObject("WinNT://localhost/"&strLocalGroup&",group")
 <span style="color: #0000ff;">Set </span>objNS = GetObject("WinNT:")
 <span style="color: #0000ff;">Set </span>oDomainGroup = objNS.OpenDSObject("WinNT://" & strDomainSlashGroup & ",group", strUsername, strPassword, ADS_SECURE_AUTHENTICATION)
 oLocalAdmGroup.Add(oDomainGroup.AdsPath)
 AddToLocalGroup = Err
<span style="color: #0000ff;">End Function
</span>
```

Sometimes you just can’t find the exact snippet that you need. That happened to me today so there it is.