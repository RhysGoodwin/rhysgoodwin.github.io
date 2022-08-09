---
id: 1956
title: 'Technicolor TG589vn V2 DNS Forwarding'
date: '2013-07-16T00:20:28+13:00'
author: RhysGoodwin
excerpt: 'How to set up conditional DNS forwarding on a Technicolor TG589vn V2 VDSL Router/Modem '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1956'
permalink: /networking/technicolor-tg589vn-v2-dns-forwarding/
categories:
    - Networking
tags:
    - 'DNS forwarding'
    - 'DNS routing'
    - Technicolor
    - TG589
---

I’ve just had VDSL2 installed at home and have been setting up the new Telecom supplied Technicolor TG589vn V2 Modem. The previous modem I had, the Technicolor TG582n has some great functionality if you don’t mind diving into the CLI (and the accompanying 800+ page CLI guide!). The TG589 is no different – a basic user friendly web GUI, backed with a much more powerful CLI.

[![](/content/uploads/2013/07/technicolor-tg589vn-vdsl-front-large.jpg "technicolor-tg589vn-vdsl-front-large")](/content/uploads/2013/07/technicolor-tg589vn-vdsl-front-large.jpg)

I haven’t been able to find the CLI guide PDF for this model yet but the command set is largely the same. One notable exception is the conditional DNS forwarding configuration which has given me some trouble. The TG582n had a set of commands for ‘DNS routing’ e.g.

<span style="color: #ff0000;">\*Update\*</span>I’ve now got a copy of the manual. linked below (Thanks Dennis and Phill)

**dns server route list** and **dns server route add.**

This has changed in the TG589 to DNS ‘forwarding rules’ and DNS ‘server sets’.

So…We have a list of DNS servers in a **dnsset** which are used in order of metric (lowest metric is used first). Then we have a set of rules as to which dnsset to use in what circumstance. The rules can match on client address, DNS domain, source interface etc. it’s quite flexible.

By default there is one dnsset and one forwarding rule. The default dnsset is set ‘0’ and is typically populated with your regular ISP DNS server. The default forwarding rule has a rule index of 999 and basically says if no other rules match then use dnsset 0.

At home I want the modem to act as the DNS server for all public internet addresses but I want queries for names on my home domain to be forwarded to my Samba domain controller/DNS linux box. Doing this means I can reboot the linux box without loosing internet access.

Here’s how I set it up:

1. Telnet to the modem – No SSH 🙁
2. Add a DNS server set: (In this case my ‘dnsset’ will just have one server in it.)  
    **dns server forward dnsset add set=10 dns=192.168.22.10 metric=20 intf=LocalNetwork**
3. Add the rule to forward any queries for home.rhysgoodwin.com to be forwarded to dnsset 10.  
    **dns server forward rule add idx=20 set=10 domain=home.rhysgoodwin.com**
4. <span style="color: #ff0000;">**Finally** </span>– the bit that tripped me up for some time was the DNS server ‘response filter’ config option. I’m not sure technically what this options is for but I had to disable it before the forwarding world work.  
    **dns server config filter=disabled**

In step 3 where you create the rule these are parameters:

**idx** the index or id of the rule. I think it also implies the order of the rule. Lower index will be matched first.

**set** is the number of the dnsset to use if this rule matches.

**domain** this is the domain name to match on. If specified then the rule will only apply to queries for names on this domain. If you leave it blank then the rule will apply to all DNS queries (which match the other parmeters of the rule)

**intf** which takes an interface name e.g LocalNetwork, PPPoE, PPPoA etc. If specified it means that the rule will only apply if the DNS query comes in on the specified interface.

**source** which takes a CIDR network address e.g 192.168.22.0/24 for my entire local subnet or 192.168.22.50/32 for a single IP address. If this is specified it means that the rule will only apply if the DNS query is coming the specified address. This is useful in cases where you one pariticular device on the network which you want to use different DNS servers for.

To Delete a forwarding rule you must specify the index number exactly like this:  
**dns server forward rule delete idx 20**

Happy Forwarding!

Here’s the CLI Guide for the TG589vn:

[ TG589vn-v2CLI (1288 downloads) ](https://blog.rhysgoodwin.com/download/2467/ "Version 1.0")Also for the TG852n

[ TG582n\_CLI\_Guide\_v1.0\_public (2853 downloads) ](https://blog.rhysgoodwin.com/download/2465/ "Version 1.0")