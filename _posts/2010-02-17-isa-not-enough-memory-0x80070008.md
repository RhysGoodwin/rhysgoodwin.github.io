---
id: 458
title: 'ISA Not Enough Memory (0x80070008)'
date: '2010-02-17T17:29:40+13:00'
author: RhysGoodwin
excerpt: 'A possible cause for error 0x80070008 in Microsoft ISA server '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=458'
permalink: /windows-admin/isa-not-enough-memory-0x80070008/
categories:
    - Networking
    - 'Windows Admin'
tags:
    - '0x80070008'
    - DNS
    - ERROR_NOT_ENOUGH_MEMORY
    - 'ISA 2006'
    - 'ISA Server'
    - NAT
    - 'Not Enough Memory'
---

I came across this one today when one of our web apps in the perimeter network stopped working for external users after a switch failure on our internal network.

[![](/content/uploads/2010/02/ISANotEnoughMemory.png "ISA Not Enough Memory")](/content/uploads/2010/02/ISANotEnoughMemory.png)

We run split DNS so if you ask an internal DNS server for the IP address of <span style="text-decoration: underline;">webapp.ourdomain.com</span> it will tell you the private address of the perimeter webserver but if you ask a DNS server on the internet you will get the public address which is NAT’d to the ISA server which publishes the app. Now if you ask a DNS server in the perimeter network he will forward the DNS request to an internal DNS server. If the internal DNS server is unavaliable the perimeter server will use recursion to resolve the address and ultimatily end up resolving and caching the public address of the webapp. By now you can probably guess what happens.

1. Internet user connects <span style="text-decoration: underline;">webapp.ourdomain.com</span> which is resolved to **203.271.47.15**
2. The connection is NAT’d by our external hardware firewall and received by the ISA web listener / publishing rule.[![](/content/uploads/2010/02/PublishWebAppTo.png "PublishWebAppTo")](/content/uploads/2010/02/PublishWebAppTo.png)
3. The ISA server resolves the name in the “To” section of the web publishing rule using a perimeter DNS server. The address is from the internal domain (ourdomain.com) so the perimeter DNS server tries to forward the request to an internal DNS server, this fails so the perimeter DNS server uses recursion to resolve the name and returns the public internet address instead of the private address of the web server in the perimeter network. We now have a loop which results on the above error being logged.

There are a couple of ways to deal with this.

1. Disable recursion on the domain forwarding in the DNS server settings:[![](/content/uploads/2010/02/DontUseRecursion.png "Dont Use Recursion")](/content/uploads/2010/02/DontUseRecursion.png)
2. Explicitly specify the IP address in the “To” section of the publishing rule.

Another one of those “*Only in this exact and unlikely situation*” type posts but oh well!