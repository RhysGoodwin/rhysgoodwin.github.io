---
id: 1075
title: 'Fingerprint Logon with hp ProtectTools -Fixed'
date: '2011-03-12T15:39:07+13:00'

excerpt: 'I had issues getting Windows fingerprint logon working with HP Protect tools, here are the steps I took to get it working.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1075'
permalink: /windows-desktop/fingerprint-logon-with-hp-protecttools-fixed/
categories:
    - 'Windows Desktop'
tags:
    - biometric
    - fingerprint
    - HP
    - logon
    - protecttools
    - 'Windows 7'
---

[![](/content/uploads/2011/03/ProtectTools.jpg "ProtectTools")](/content/uploads/2011/03/ProtectTools.jpg)

I just got a new hp 8440P [EliteBook](http://www.amazon.com/gp/product/B004RCUIQI/ref=as_li_tf_tl?ie=UTF8&tag=blogrhysgoodw-20&linkCode=as2&camp=217145&creative=399373&creativeASIN=B004RCUIQI)![](https://www.assoc-amazon.com/e/ir?t=blogrhysgoodw-20&l=as2&o=1&a=B004RCUIQI&camp=217145&creative=399373)(Windows 7×64) and wanted to use fingerprint logon. To cut a long story short:

1. Installed latest ProtectTools security manager v.5.1.1.744  
    <span style="color: #0000ff;">Result:</span> ProtectTools is installed but no fingerprint options.
2. Installed Validity Fingerprint Sensor Driver v.4.0.15.0  
    <span style="color: #0000ff;">Result:</span> Fingerprint options are visible in ProtectTools and fingerprint enrolment is successful but <span style="color: #ff0000;">no option at windows logon to unlock with fingerprint</span>
3. Un-installed ProtectTools v.5.1.1.744
4. Installed ProtectTools v.5.0.4.669 (The version I have on my other Elitebook which works)  
    <span style="color: #0000ff;">Result:</span> Windows logon now displays the swipe to logon option but when you swipe it throws an error: “<span style="color: #ff0000;">the system cannot find the specified file</span>“.
5. Installed ProtectTools v.5.1.1.744 again over the top of v.5.0.4.669 which upgrades v.5.0.4.669 to v.5.1.1.744  
    <span style="color: #0000ff;">Result:</span> **<span style="color: #339966;">Everything is working as it should!</span>**