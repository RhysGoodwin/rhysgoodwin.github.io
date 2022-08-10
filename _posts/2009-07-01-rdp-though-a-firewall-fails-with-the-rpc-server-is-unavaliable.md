---
id: 310
title: 'RDP through a firewall fails with: &#8220;The RPC server is unavaliable&#8221;'
date: '2009-07-01T11:19:48+13:00'

excerpt: 'Trying to RDP to a Windows 2003 server behind a firewall using an account from a trusted domain fails with an RPC error. An easy workaround without opening up RPC on the firewall!'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=310'
permalink: /windows-admin/rdp-though-a-firewall-fails-with-the-rpc-server-is-unavaliable/
categories:
    - 'Windows Admin'
tags:
    - 'Domain Trust'
    - 'Event 1219'
    - Firewall
    - IgnoreRegUserConfigErrors
    - ISA
    - RDP
    - 'Remote Desktop'
    - RPC
    - 'RPC error'
    - 'Terminal Services'
---

This is just a quick one…

When trying to logon to Windows server 2003 via remote desktop you receive the following message:

*The system cannot log you on due to the following error:  
The RPC server is unavailable.  
Please try again or consult your system administrator.*

*[![RCPError](/content/uploads/2009/07/RCPError.JPG "RCP Error")](/content/uploads/2009/07/RCPError.JPG)*

You will also receive the following event in the target server’s application event log:

*Event ID: 1219  
Logon rejected for Domain\\User. Unable to obtain Terminal Server User Configuration. Error: The RPC server is unavailable.*

[![Event ID 1219](/content/uploads/2009/07/WinLogonError.JPG "Event ID 1219")](/content/uploads/2009/07/WinLogonError.JPG)

There are a number of reasons you might see this message but in my case it was because the server I was connecting to was behind a firewall and in different domain to the one which my account was in.

When you logon via RDP, “Terminal Services” will contact the domain which your account is in to query terminal services information about your account e.g. profile path. It does this using RPC to a domain controller.

In my case the server concerned was in the perimeter network and there was no way I was going open RPC on the firewall to allow it to talk to an internal DC. And since the purpose for RDP to this server was purely for administration I really didn’t care if it couldn’t get my profile info from AD.

Fortunately there is a workaround as described in this Microsoft article, actually the article refers to a different problem, but the workaround is the same.

<http://support.microsoft.com/kb/815266>

1. Locate the following registry subkey:  
    HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server
2. Create a new DWORD called **IgnoreRegUserConfigErrors**
3. Give it a value of **1**

Done! I might consider creating a group policy preference to implement this across all the servers in the perimeter domain.