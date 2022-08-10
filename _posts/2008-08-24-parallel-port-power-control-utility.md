---
id: 115
title: 'Parallel Port Power Control Utility'
date: '2008-08-24T22:28:43+13:00'

excerpt: 'A tidy system tray tool to control AC devices that are switched by a parallel port or print server. Supports multiple networked parallel ports / printer servers.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=115'
permalink: /hardware/parallel-port-power-control-utility/
categories:
    - Electronics
    - Hardware
tags:
    - 'home automation'
    - inpout
    - inpout32.dll
    - 'parallel port'
    - 'power control'
    - 'print server'
    - 'solid state relay'
---

<span style="color: #ff0000;">\*Update I’ve released a new version of Power Control \*</span>

<span style="color: #ff0000;">\*Check out the [new Post Here](https://blog.rhysgoodwin.com/windows-desktop/powercontrol-home-automation/)\*</span>

As promised when I posted about the [print server power control Hack](https://blog.rhysgoodwin.com/2008/06/06/print-server-power-control-hack/), I’ve finally gotten around to writing a little windows app to control devices from the system tray. The utility is called PowerTray, it can control local devices or devices connected to a networked computer as long as they are also managed by PowerTray.

[![PowerTray Screenshot](/content/uploads/2009/04/screenshots1.jpg "PowerTray Screenshot")](/content/uploads/2009/04/screenshots1.jpg)

PowerTray can also integrate with [MyPowerControl in MediaPortal](http://www.team-mediaportal.com/files/Download/Plugins/Utilities/MyPowerControl/) HTPC system. If you’re using this with MediaPortal then only install either **PowerTray** <span style="text-decoration: underline;">or</span> the **MyPowerControl** plug-in in a single computer not both!

Source is available in the [MediaPortal plug-ins SVN](http://mp-plugins.svn.sourceforge.net/viewvc/mp-plugins/trunk/plugins/MyPowerControl/)  
<span style="text-decoration: underline;">Version History</span>

0.1.9.0 (2009-03-21)  
-Lots of little bug fixes and better error handling

0.1.7.0 (2008-08-24)  
-Initial release

[](/post-content/PowerTray/PowerTraySetup.msi "Download PowerTray 0.1.9.0")0.3.0 – (Read about and download the [new version of PowerControl](https://blog.rhysgoodwin.com/windows-desktop/powercontrol-home-automation/))