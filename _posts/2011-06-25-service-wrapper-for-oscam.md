---
id: 1378
title: 'Service Wrapper for oscam'
date: '2011-06-25T16:55:17+13:00'
author: RhysGoodwin
excerpt: 'Windows service wrapper for oscam.  '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1378'
permalink: /windows-desktop/service-wrapper-for-oscam/
categories:
    - 'Windows Desktop'
tags:
    - acamd
    - cam
    - mdapi
    - oscam
    - smartcard
---

Ok this is one of those *“if you need it, you’ll know what I’m talking about posts”*!

[![](/content/uploads/2011/06/svcmgr.jpg "Services Manager")](/content/uploads/2011/06/svcmgr.jpg)

I recently started using oscam and being that I don’t like sever applications that need to run in the foreground I wrote a small windows service wrapper to handle oscam for me.

Just drop it in your oscam folder and install it by running **oscamSVC -install**

Using oscam as a system service also overcomes the issue that some people have found with some USB card readers such as the Omnikey disconnecting or ejcecting when a remote desktop connection is made to the PC running oscam**.**

[![](/content/uploads/2011/06/ConfigTool.jpg "Config Form")](/content/uploads/2011/06/ConfigTool.jpg)

**Changes**

0.3.5.0 – Current version

- Added settings form (Loaded by starting oscamSVC.exe without any parameter)
- Added start-up delay option  
    Use this option if you see card detection errors in your oscam log. I did – even though I made oscamSVC dependent on the smartcard service.  
    Adding the delay just lets Windows start a bit more which for what ever reason seems to help. It will be dependent on your reader.  
    During the delay the service is kept in the ‘starting’ state so you can still add service dependencies using regedit.  
    e.g. MediaPortal TVServer depends on oscamSVC.
- Changed command line options from slash to dash  
    (i.e oscamSVC.exe -install instead of oscamSVC.exe /install)

0.3.0.0

- Initial public release

Requires .NET 2.0

Enjoy.
### Download
[ oscamSVC "Version 0.3.5.0" ](/content/uploads/downloads/2011/06/oscamsvc.exe)