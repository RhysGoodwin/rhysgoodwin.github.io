---
id: 2161
title: 'Eventghost plugin for Panasonic TV Control'
date: '2014-11-26T18:10:31+13:00'
author: RhysGoodwin
excerpt: 'VieraControl - A simple Eventghost plug-in to control Panasonic TVs over IP'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=2161'
permalink: /htpc/eventghost-plugin-for-panasonic-tv-control/
categories:
    - HTPC
tags:
    - eventghost
    - 'home automation'
    - 'ip control'
    - 'panasonic viera'
---

Here is an Eventghost plug-in that I threw together to control Panasonic TVs. This is based on sending soap messages just like the Panasonic Android/iphone remote apps do.

It’s fairly rudimentary and I’ve only tested it on a VT30 plasma. Let me know if you find any issues and I’ll try to fix them. Feature requests are welcome but unlikely to be implemented quickly.

[![VieraControl](/content/uploads/2014/11/VieraControl.jpg)](/content/uploads/2014/11/VieraControl.jpg)

![VieraControlConfig](/content/uploads/2014/11/VieraControlConfig.jpg)

Unfortunately there aren’t many discrete commands and to accomplish a lot of things would require automation of menu navigation which may be error prone. Still, better than nothing.

Power on via network is not possible on the VT30 (I use CEC bus instead). I believe that it is possible on ST50, VT50, ST60, VT60 etc.

Just drop the **VieraControl** folder within the zip file into the Eventghost plugins folder.

Enjoy.

#### Download
[ VieraControl  ](/content/uploads/downloads/2014/11/VieraControl0.0.2.zip "Version 0.0.2")