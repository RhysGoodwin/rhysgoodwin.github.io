---
id: 537
title: 'PPTP VPN &#038; Citrix client drive mapping fails behind belkin router'
date: '2010-07-05T19:00:59+13:00'

excerpt: 'Some Belkin routers break PPTP VPNs and Citrix client drive mapping '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=537'
permalink: /windows-admin/pptp-vpn-citrix-client-drive-mapping-fails-behind-belkin-router/
categories:
    - 'Windows Admin'
tags:
    - Belkin
    - CDMAllowed
    - Citrix
    - 'client drives'
    - 'drive mapping'
    - ICA
    - 'local drives'
    - PPTP
    - VPN
---

[![Pic:Belkin F5D8635](/content/uploads/2010/07/F5D8635.jpg "Belkin F5D8635")](/content/uploads/2010/07/F5D8635.jpg)I came across this today. I’m not sure what’s causing it or how to fix it and probably won’t get a chance to dig any deeper but thought I’d comment anyway.

A Citrix web client sitting behind a Belkin F5D8635 wireless/ADSL router doesn’t get his local client drives mapped in his session. This is seen with several Citrix client versions, server version MPS 4.0 an 4.5.

This has been observed on two separate Belkin routers of the same model.

I’ve also noticed that trying to establish a Microsoft PPTP VPN connection from a client behind this router also fails.

**<span style="color: #ff0000;">\* Update \*</span>**

Belkin has now has a **pre-**release firmware which they say fixes the PPTP problem. I haven’t tested it yet so can’t confirm whether it fixes the Citrix drive mapping issue.