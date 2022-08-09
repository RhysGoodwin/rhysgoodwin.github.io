---
id: 244
title: 'Active Directory and Kerberos SPNs Made Easy!'
date: '2009-04-07T01:26:18+13:00'
author: RhysGoodwin
excerpt: 'A very clear description of why (and how) we set Kerberos SPNs in active directory. '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=244'
permalink: /windows-admin/active-directory-and-kerberos-spns-made-easy/
categories:
    - 'Windows Admin'
tags:
    - 'kerberos spn'
    - 'kerberos ticket'
    - 'service account'
    - 'service principal name'
    - setspn
---

There are a lot of articles out there on setting up Kerberos **S**ervice **P**rincipal **N**ames but today I’m going to make it simple. Bear with me as I start off with the basics; by the end of the post it will all be very clear.

Throughout this post I’ll make reference to a scenario of a client computer connecting to an SQL server called **sql1.domain.com** however the same applies for any service, for example a web server where the client connects via HTTP.

The SQL server service is running under a domain service account called “**domain\\SQLSVC**“. No SPNs have been set yet.

## **The Basics**

Active directory user and computer accounts are objects in the active directory database. These objects have attributes. Attributes like *Name* and *Description.*

Computer and User accounts are actually very similar in the way they operate on a Windows domain and they both share an attribute called **ServicePrincipalName**. An account object can have multiple **ServicePrincipalName** attributes defined**.**

The setspn.exe tool manipulates this attribute. That’s all it does.

## **The Failure**

<span style="color: #339966;">The client wants to access the SQL server so he asks his domain controller: *“Please may I have a ticket for accessing **MSSQLSvc/sql1.domain.com”***</span>

<span style="color: #ff6600;">Now the domain controller asks the active directory database: “*Give me the name of the account object who’s ServicePrincipalName is **MSSQLSvc/sql1.domain.com**“*</span>

<span style="color: #0000ff;">The active directory database replies: *“Sorry, there are no account objects with that ServicePrincipalName”*</span>

<span style="color: #ff6600;">So the domain controller asks the active directory database again: *“Ok then, give me the account object who’s ServicePrincipalName is **HOST/sql1.domain.com**“*</span>

**All** computer accounts have, by default ServicePinciaplName attributes set to:  
**HOST/*\[computername\]*** and **HOST/*\[computername\].\[domain\]***

<span style="color: #0000ff;">So the active directory database replies to the domain controller: *“The account object that has that ServicePrincipalName is sql1.domain.com’s **computer account**“*</span>

The domain controller now creates a ticket that only the computer account of **sql1.domain.com** can read. He gives the ticket to the client.

<span style="color: #339966;">The client goes to the SQL service on sql1.domain.com and says “*here is my ticket, may I come in?”*</span>

The SQL service will attempt to read the ticket. The problem is, the SQL service is not running under the computer account; it is running under a domain service account. It can not read the ticket; the ticket is only intended for the computer account of sql1.domain.com. Authentication fails (falls backto NTLM).

## **The Fix**

Now lets run the **setspn.exe** tool to manipulate the **ServicePrincipalName** attribute of the SQL service account.

[![Command promt screenshot showing setspn](/content/uploads/2009/04/setspncmd.jpg "setspn.exe")](/content/uploads/2009/04/setspncmd.jpg)

```
<pre style="text-align: left;">setspn -a MSSQLSvc/sql1.domain.com domain\SQLSVC
```

We will also add **sql1** (without the domain name) in case we want to access the the server without the domain name appended.

```
<pre style="text-align: left;">setspn -a MSSQLSvc/sql1 <strong>domain\SQLSVC</strong>
```

Now run through the scenario again and this time notice that the domain controller will return a ticket that the SQL server service account can read.

Obviously this is heavily paraphrased but hopefully it helps you understand the reason for setting the SPN attribute on the account that runs a given service. Of course if the service runs under the local **NetworkService** or **LocalSystem** account then everything will just work because these local accounts represent the computer account in active directory.