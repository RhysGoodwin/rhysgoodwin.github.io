---
id: 1923
title: 'AVR Control Without Ugly IR Bugs'
date: '2013-06-03T22:42:52+13:00'

excerpt: 'A cheap way to get control of your home theater receiver without sticking an ugly IR bug to the outside.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1923'
permalink: /htpc/avr-control-without-ugly-ir-bugs/
categories:
    - HTPC
tags:
    - automation
    - blaster
    - control
    - emitter
    - infrared
    - yamaha
---

A couple of months ago I bought a secondhand Yamaha receiver (RX-V371). My plan was to finally do away with infrared blasters stuck to the outside of all my home theater gear. The plan was to do all the control with HDMI CEC. I was already controlling my TV using the[ Kwikwai HDMI CEC adapter](https://blog.rhysgoodwin.com/htpc/hands-on-with-kwikwai-hdmi-cec/) which I reviewed a couple of posts ago.

[![](/content/uploads/2013/06/Yamaha_RX-V371_front.jpg "Yamaha_RX-V371_front")](/content/uploads/2013/06/Yamaha_RX-V371_front.jpg)

Unfortunately I haven’t been able to find the necessary Yamaha HDMI CEC commands to control things like surround mode and DSP settings. Chances are these commands just don’t exist. I did email Yamaha but they didn’t even respond. The most I could get out of CEC on the Yamaha was power on/off and input select.

I couldn’t bring myself to stick an IR blaster to the beautiful face of this fine receiver. Equally unappealing was the idea of shelling out cash for a receiver with Ethernet or RS-232 control.

Here is my compromise – putting the IR blaster inside the receiver. It’s not rocket science but here you go:

[![](/content/uploads/2013/06/TopDown.jpg "TopDown")](/content/uploads/2013/06/TopDown.jpg)

[![](/content/uploads/2013/06/IRBlaster.jpg "IRBlaster")](/content/uploads/2013/06/IRBlaster.jpg)

[![](/content/uploads/2013/06/StickIRBlaster.jpg "StickIRBlaster")](/content/uploads/2013/06/StickIRBlaster.jpg)

After locating the IR sensor I removed the self-adhesive backing on the IR blaster and stuck it to the PCB with the aid of a bamboo skewer.

[![](/content/uploads/2013/06/CableExit.jpg "CableExit")](/content/uploads/2013/06/CableExit.jpg)

The only thing left to do was to cut the the 3.5mm mono plug off the blaster, pass the cable through a small hole at the back of the receiver and then re-attach the 3.mm mono plug using a soldering iron and some heat-shrink tubing.

The Infrared emitter (blaster) is connected to a Microsoft USB Infrared Receiver/Transmitter. I’m using my own home brew c# .NET application to do the automation but there are a number of options, Girder, HIP, EventGhost etc.

The result – a reasonable level of control, no ugly IR bug visible (I can’t even see it flashing) and zero cost.