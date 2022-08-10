---
id: 160
title: 'MOSS 2007 SSRS WMI provider error'
date: '2009-03-22T23:37:32+13:00'

excerpt: '“Unable to connect to the Report Server WMI provider“. Another possible cause / Fix for this error.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=160'
permalink: /windows-admin/moss-2007-ssrs-wmi-provider-error/
categories:
    - 'Windows Admin'
tags:
    - MOSS
    - sharepoint
    - SSRS
    - 'WMI error'
---

Setting up SQL Server Reporting Services in SharePoint integration mode and you turn up with this error when you try to *“Grant Database Access”* from within Central Administration:

**<span style="color:#ff0000;"><span style="color:#808080;">“</span>Unable to connect to </span><span style="color:#ff0000;">the Report Server WMI provider<span style="color:#808080;">“</span></span>**

**<span style="color:#ff0000;"><span style="color:#808080;">[![grantdberror](/content/uploads/2009/04/grantdberror.jpg?w=300 "grantdberror")](/content/uploads/2009/04/grantdberror.jpg)</span></span>**

If you’ve got an ISA firewall between your MOSS central admin and your SSRS server you’ll need to turn off *“Strict RPC Compliance*” just while central admin performs the *“Grant Database Access”* operation .

[![isarpcsettings](/content/uploads/2009/04/isarpcsettings.jpg "isarpcsettings")](/content/uploads/2009/04/isarpcsettings.jpg)

Hope this saves someone some time. Not a very common scenario I guess.