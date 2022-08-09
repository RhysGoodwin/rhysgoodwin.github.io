---
id: 1850
title: 'Hands on with Kwikwai HDMI CEC'
date: '2013-04-21T15:08:28+13:00'
author: RhysGoodwin
excerpt: 'Kwikwai - Powerful HDMI CEC Home Theater Automation '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1850'
permalink: /htpc/hands-on-with-kwikwai-hdmi-cec/
categories:
    - HTPC
tags:
    - automation
    - cec
    - hdmi
    - HTPC
    - kwiawai
---

Over the last few months I’ve had the opportunity to play with a very cool toy and thought I’d take some time to share it here. The Kwikwai is a powerful little tool made by Swiss company [Incyma](http://www.incyma.com/). It enables complete access to the HDMI-CEC bus. If you haven’t heard of HDMI-CEC it’s probably because it’s normally re-labelled by manufacturers. Anynet+ (Samsung); Aquos Link (Sharp); BRAVIA Sync (Sony); VIERA Link (Panasonic) etc.

CEC stands for **Consumer Electronics Control** and it allows various home entertainment components to talk to each other. For example when you switch on your Blu-ray player your TV and amp will turn on and switch to the correct inputs. Or when you turn your TV off the other HDMI connected devices will also turn off.

While this might all sound great in theory, in practice it can be a hit and miss. Different manufactures implement their own flavour of CEC and devices from different manufacturers don’t always play nicely together.

My interest in CEC was not so much in the interaction between devices and more in direct control and automation of each individual device using my HTPC. In fact I don’t even have a blu-ray player or set-top-box. Everything is done through the HTPC. I have a bit of an obsession with having a single remote to control everything with as few buttons as possible. Anyone should be able to pick up the remote, press power and be presented with an intuitive interface (in my case MediaPortal).

While there are plenty video cards that offer HDMI they don’t yet offer communication on the CEC bus. That’s where Kwikwai comes in.

[![](/content/uploads/2013/04/KwikwaiFront.jpg "Kwikwai Front")](/content/uploads/2013/04/KwikwaiFront.jpg)

On the front there are 4 indicator LEDs and two HDMI ports which allow the Kwikwai to be placed ‘in-line’ between two devices e.g. Blu-ray player and TV. It doesn’t matter which device connects to which port since the Kwikwai is completely transparent to the devices connected to it. You don’t have to connect it in-line you could just connect to any spare HDMI port on your TV or Amp – everything that goes onto the CEC bus is broadcast across all ports.

[![](/content/uploads/2013/04/KwikwaiRear.jpg "KwikwaiRear")](/content/uploads/2013/04/KwikwaiRear.jpg)

On the rear of the Kwikwai there are three connectivity options. Ethernet, RS232 and USB. The USB interface is used for power and also for communication (via USB to RS232). You can power the Kwikwai either from your PC or from any other 5V USB power supply. I’ve really only used the network interface so far.

The Kwikwai is not only great for home theatre automation it’s also a powerful HDMI CEC diagnostic tool and that’s the primary use for the web interface which can be accessed by pointing your browser at [http://kwikwai.local ](http://kwikwai.local)

[![](/content/uploads/2013/04/WebIf.jpg "WebIf")](/content/uploads/2013/04/WebIf.jpg)

While the web interface provides diagnostics, configuration, and a firmware update facility it’s not ideal for automation. For that we can either use the command line directly or use the API for developing custom software. Most HTPC users will opt for using the command line but if you’ve got some basic c# .NET skills using the API is quite easy.

There is also some sample Python code on the Kwikwai website which would make it pretty easy to implement an Eventghost plug-in, however I was able to get the Kwikwai working in Eventghost by using the existing ZoomPlayer plug-in which allows simple RAW TCP commands to be sent.

[![](/content/uploads/2013/04/EventGhost1.jpg "Event Ghost Zoom Player Plug-in")](/content/uploads/2013/04/EventGhost1.jpg)

Simply enter the Kwikwai address and port number

[![](/content/uploads/2013/04/EventGhost.jpg "EventGhost")](/content/uploads/2013/04/EventGhost.jpg)

To send commands to the Kwikwai create a new Zoomplayer ‘Raw Command’ action. For example:

cec:send A FF:36 This will broadcast the ‘Power Off” command to all devices on the CEC bus.

The command syntax can be found on the Kwikwai web site [here](http://downloads.kwikwai.com/k1-100/rev529/cli.html). And the [CEC-O-MATIC](http://www.cec-o-matic.com/) is a great online tool to help you build up CEC commands.

**Conclusion**

The Kwikwai is a very handy device which enables easy automation of home entertainment components without the need to stick ugly infra-red senders to your equipment. At the moment it can be hard to get hold of vendor specific commands to perform more complex control but hopefully that will change over time.

There are two Kwikwai models available. For a full diagnostics solution the K-100 is the more expensive model. For automation the more basic K-090 will be more than adequate. Both models include all the connectivity options.

The only two areas I can see room for improvement in the Kwikwai are:  
1\) The colour! The Kwikwai looks kind of cool and is very well built but it doesn’t blend in very well with most home theatre gear.  
2\) It would be nice to see a firmware update that enables the Kwikwai to emulate a ‘player’ device on the HDMI bus so that other devices could become aware of it.

Check out the Kwikwai at <http://www.kwikwai.com>