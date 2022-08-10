---
id: 695
title: 'PowerControl Home Automation'
date: '2010-11-10T10:33:37+13:00'

excerpt: 'Control external devices using a parallel port, network print server or K8055 USB board. Now with network control support! Free download. '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=695'
permalink: /hardware/powercontrol-home-automation/
categories:
    - Hardware
tags:
    - 'home automation'
    - inpout32.dll
    - K8055
    - 'parallel port'
    - 'power control'
    - powertray
    - 'print server'
    - 'solid state relay'
    - ssr
---

You might have already seen my PowerTray tool in an earlier post. I’ve made some big changes to the tool so I thought it warranted a brand new post.

The Power control suit allows you to control external devices in your home. PowerControl supports the following output controllers:

- Local Parallel Port
- Network Print Server (See my [print server page](https://blog.rhysgoodwin.com/hardware/print-server-power-control-hack/))
- [K8055 USB board](http://www.velleman.eu/distributor/products/view/?country=be&lang=en&id=351346) (Up to 4 boards)

Using PowerControl with one of the above and a simple relay or solid-sate-relay arrangement you can control any appliance in the house.

PowerControl operates in a server/client architecture. It consists of the following components:

**PowerControl Server Service** *(Server)*

This windows service is responsible for controlling hardware, whether it be attached via Parallel port, print server or K8055. The PowerControl Config tool is used to add and remove devices which are controlled by this service. The PowerControl service can be managed from any PC on the network using the PowerControl Config tool (All tasks except configuring the local service instance, this must be done by running the config tool locally on the PC which runs the service).

**PowerTray** *(Client)*

PowerTray provides a system tray menu which is used to interact with a PowerControl server to turn devices on and off. It can interact with any number of PowerControl servers and can be run on any number of PCs on the local network. This means multiple PCs have access to the same devices. The PowerControl service maintains the state of these devices and updates each instance of PowerTray. For example You might turn on a Lamp from PC1 then turn it off later from PC2.

**PowerCmd** *(Coming soon!)*

PowerCmd is a command line tool which allows you to interact with a PowerControl service. This allows you control devices from other programs and scripts.

[![](/content/uploads/2010/11/architecture.jpg "PowerControl Architecture")](/content/uploads/2010/11/architecture.jpg)

**Version History**  

0.1.7.0 (2008-08-24) (PowerTray)

- Initial release

0.1.9.0 (2009-03-21) (PowerTray)

- Lots of little bug fixes and better error handling

0.3.0.0 (2010-11-10) (PowerControl) *Current release*

- Added support for K8055 USB
- Added support for Windows 7
- Split service config from PowerTray client

**Download**  
[ PowerControlSetup.msi ](/content/uploads/downloads/2010/11/PowerControlSetup.msi)