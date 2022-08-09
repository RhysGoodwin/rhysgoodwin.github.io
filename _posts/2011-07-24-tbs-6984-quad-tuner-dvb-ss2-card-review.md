---
id: 1421
title: 'TBS 6984 Quad Tuner DVB-S/S2 Card (Review)'
date: '2011-07-24T19:00:44+13:00'
author: RhysGoodwin
excerpt: 'Review of the TBS 6984 Quad Tuner DVB-S/S2 PCIe Satellite TV Card'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1421'
permalink: /htpc/tbs-6984-quad-tuner-dvb-ss2-card-review/
image: /wp-content/uploads/2011/07/PCBFront-90x90.jpg
categories:
    - HTPC
tags:
    - '6984'
    - dvb-s
    - dvb-s2
    - HTPC
    - mediaportal
    - tbs
---

#### Intro

In my last post I showed you my [RG6 coax patch panel](https://blog.rhysgoodwin.com/home-diy/structured-cable-at-home-f-patch-panel/) build which included cabling up the 4 outputs of my LNB. Well there was a reason I risked life and limb on the roof in high winds.

TBS Technology of Shenzhen China have only been making TV cards for about 5 years but they’re starting to build a good reputation amongst HTPC enthusiasts, and for good reason – as you’ll see the 6984 is a solid performer. This review will focus on using the TBS 6984 with MediaPortal TV Server.

 [![](/content/uploads/2011/07/PCBFront.jpg "PCBFront")](/content/uploads/2011/07/PCBFront.jpg)

The TBS6984 really is the grand-daddy of DVB-S cards! It’s a DVB-S/S2 PCI Express card with 4 tuners allowing you capture from 4 different satellite transponders simultaneously. At $249USD the price is right. That’s about $62 per tuner – considerably cheaper than buying 4 separate DVB-S2 cards and much more convenient.

[![](/content/uploads/2011/07/PCBRear.jpg "PCBRear")](/content/uploads/2011/07/PCBRear.jpg)

TBS list the following specifications:

<span style="color: #0000ff;">Receiving Frequency: 925~2175 MHz Tuning Range</span>  
<span style="color: #0000ff;">Input Level: -69~23dBm</span>  
<span style="color: #0000ff;">4x Advanced DVB-S2/DVB-S 8PSK QPSK Demodulator</span>

**<span style="color: #0000ff;">Symbol Rates: </span>**  
<span style="color: #0000ff;">DVB-S QPSK: 1-45 Msps</span>  
<span style="color: #0000ff;">DVB-S2 8PSK/QPSK: 2-36 Msps</span>

**<span style="color: #0000ff;">Code Rates:</span>**  
<span style="color: #0000ff;">DVB-S: 1/2, 2/3, 3/4, 5/6, 7/8</span>  
<span style="color: #0000ff;">DVB-S2 QPSK: 1/2, 3/5, 2/3, 3/4. 4/5. 5/6, 8/9, 9/10</span>  
<span style="color: #0000ff;">DVB-S2 8PSK: 3/5, 2/3. 3/4. 5/6. 8/9, 9/10</span>

I’m not going to pretend that I could explain all the technical aspects of these specs, but suffice to say this card will handle pretty much anything you can throw at it.

#### What’s In The Box

- The PCIe Card
- A driver mini-CD
- Infrared Remote Control
- Infrared Receiver Cable
- Power Cable

<div style="text-align: center;">[![](/content/uploads/2011/07/PackageContents1-1024x663.jpg "PackageContents")](/content/uploads/2011/07/PackageContents1.jpg)</div>The build quality of the card is excellent – all the soldering looks clean and solid, and the components are well aligned. The bracket has labels for the tuners ‘A’ through ‘D’ stamped on it – a nice touch. The chipset consists of the following:

- 2x Trident CX24132 Dual DVB-S2 Tuner
- 2x Trident CX24117 Dual DVB-S2 Demodulator
- 2x Intersil ISL6422B Dual Output LNB Supply and Control Voltage Regulator
- 1x Trident SAA7160E PCI Express Audio/Video Capture Bridge

<div>[![](/content/uploads/2011/07/PCBClose-1024x768.jpg "PCBClose")](/content/uploads/2011/07/PCBClose.jpg)</div>The remote is pretty basic; but let’s be honest, if you’re looking for a quad tuner card you’re probably an HTPC enthusiast in which case you’ll already have an advanced remote. If not you’ll need to get one! It has TV and navigation buttons, but lacks buttons for the advanced features you’d typically find in media centre packages like MediaPortal. That said, it will do just fine to get you up and running with basic TV software.

As for the driver CD, I haven’t even put it in my PC. Personally I never use the driver CDs which come with any hardware. I prefer to go straight to the web and download the very latest version. It would be nice if TBS released a white-box version of this product, which included just the card and the power cable.

TBS state that the additional power cable is only required when you need extra current for driving things like dish positioning motors and *some* LNBs. In general, you shouldn’t require it which is good – the less cables floating around the better when it comes to an HTPC that you’re trying to keep cool with a minimum of fans.

<span class="Apple-style-span" style="font-size: 18px; font-weight: bold;">Installation</span>

I’d love to go into great detail about the installation but there really isn’t much to say. The hardware side is obvious – unplug your PC and install the card into a spare PCIe 1x slot.

As for the driver, TBS keeps it simple which I really appreciate. You don’t have to run an installer (although there is one). You can simply let Windows detect the card and then tell it where to find the latest driver files and the device installs without any fuss. I wish more manufacturers would take this simple clean approach. With an installer, you don’t really know what you’re getting and what’s being changed on your system. I’m running Windows 7 x64. Once the driver is installed, you’ll see a single “TBS 6984 Quad DVBS/S2 BDA Tuners” device listed in device manager. The driver is a BDA driver, which means it conforms to Microsoft’s broadcast driver architecture so the card will be compatible with any TV software which supports BDA devices.

[![](/content/uploads/2011/07/DeviceManagerProperties.png "DeviceManagerProperties")](/content/uploads/2011/07/DeviceManagerProperties.png)

<span class="Apple-style-span" style="font-size: 18px; font-weight: bold;">MediaPortal</span>

MediaPortal is a free and open source media centre package for Windows.

<div class="wp-caption aligncenter" id="attachment_1467" style="width: 563px">[![MediaPortal TV Guide](/content/uploads/2011/07/MPTVGuide1.jpg "MPTVGuide")](/content/uploads/2011/07/MPTVGuide1.jpg)MediaPortal TV Guide

</div><div class="wp-caption aligncenter" id="attachment_1466" style="width: 563px">[![MediaPortal Home](/content/uploads/2011/07/MPHome.jpg "MPHome")](/content/uploads/2011/07/MPHome.jpg)MediaPortal Home

</div>You can get a wealth of information and support at the [Team-MediaPortal](http://www.team-mediaportal.com/) site, but these are the basic components that you’ll need to get TV up and running with the TBS 6984. The remainder of this review will focus on the TV Server component of MediaPortal.

- MediaPortal – this is the main front-end application. You can have this installed on as many PCs around the house as you like
- TV Server – this is a Windows service which manages all TV streaming and recording. It can be on the same or a different PC to the MediaPortal application
- TV Client Plug-in – this is a plug-in component to MediaPortal which connects it to the TV Server

Once you’ve got the driver installed TV Server will detect the card. You’ll have to restart the TV Service and TV Server configuration tool if they are already running.

One thing I really like about this card is the way it identifies itself. In the Windows device manager it just shows a single device, but once you open up the TV Server configuration tool you’ll see all 4 tuners and they’re actually labelled A, B, C and D – unlike some other dual cards I’ve seen, which just show 2 identical tuners so you can’t tell which one is which.

[![](/content/uploads/2011/07/Cards.png "Card List")](/content/uploads/2011/07/Cards.png)

Scanning speed is impressive – just over 6 minutes to scan 41 transponders. Both DVB-S and DVB-S2 channels are found correctly.

[![](/content/uploads/2011/07/Scanning.png "Scanning")](/content/uploads/2011/07/Scanning.png)

Now the part you’ve been waiting for – recording 4 channels at once. In fact, with MediaPortal TV Server you can record even more than that because it allows you to record all the channels on a given transponder at the same time. The TBS 6984 can tune into 4 separate transponders, so if each of those transponders carries 6 channels that would mean you could record 24 channels simultaneously! Below you can see I’m receiving 12 channels quite happily and the 6984 doesn’t skip a beat! *“Just try that Windows 7 Media Center!”*

The driver seems to report the signal quality and strength much more accurately than a lot of other cards I’ve seen, and also updates these quite frequently – which is great.

[![](/content/uploads/2011/07/Timeshifting.png "Timeshifting")](/content/uploads/2011/07/Timeshifting.png)

**Channel Change Speed**

The most common question I hear when discussing various TV cards with HTPC enthusiasts is *“How fast can it change channels?”.* There are a number of things that can affect this – system hardware, TV card, TV card driver, TV software, media codecs, etc. It also depends on where you take the measurement. The following results are taken from the TV Server logs, and indicate the time it takes for the TV card to switch channels.

```
Start DVB-S Channel (Same channel/transponder for each tune)
 1st Tune: 437ms
 2nd Tune: 218ms
 3rd Tune: 203ms
 4th Tune: 281ms
 5th Tune: 124ms
```

```
Start DVB-S Channel (Different channel/transponder for each tune)
 1st Tune: 390ms
 2nd Tune: 718ms
 3rd Tune: 531ms
 4th Tune: 796ms
 5th Tune: 609ms
```

```
Change DVB-S Channel  (Same transponder for each tune)
 1st Tune: 437ms
 2nd Tune: 453ms
 3rd Tune: 374ms
 4th Tune: 281ms
 5th Tune: 593ms
```

```
Change DVB-S Channel  (Switch transponder for each tune)
 1st Tune: 475ms
 2nd Tune: 374ms
 3rd Tune: 468ms
 4th Tune: 468ms
 5th Tune: 687ms
```

```
Change DVB-S2 Channel  (Same transponder for each tune)
 1st Tune: 234ms
 2nd Tune: 296ms
 3rd Tune: 234ms
 4th Tune: 343ms
 5th Tune: 171ms
```

```
Change DVB-S2 to DVB-S (Switch transponder for each tune)
 1st Tune: 656ms
 2nd Tune: 390ms
 3rd Tune: 718ms
 4th Tune: 656ms
 5th Tune: 687ms
```

```
Change DVB-S to DVB-S2 (Switch transponder for each tune)
 1st Tune: 2159ms
 2nd Tune: 2124ms
 3rd Tune: 2156ms
 4th Tune: 2218ms
 5th Tune: 2187ms
```

Very impressive, with all tests sub-second, except DVB-S to DVB-S2 switching which takes a little longer.

**DiSEqC**

The TBS 6984 supports DiSEqC 2.x. MediaPortal TV Server doesn’t yet support DiSEqC for this card, but I’ve spoken to a member of the MediaPortal development team who has informed me that they will be adding it soon, and has asked me to be a tester when the time comes.

#### Conclusion

All-in-all, my only criticism is that such a high-end card should be matched with a high-end remote. I think the best solution is a white box version of the product so the user can choose their own remote.

The channel change speed tests speak for themselves; that, combined with the solid driver and excellent build quality, makes the 6984 an excellent choice for anyone looking to build or expand an HTPC. In fact, unless you’re certain you won’t need more than 2 tuners, I would say just go straight for the 6984 because you’ll end up saving money in the long run.

As for TBS support – while I haven’t needed any technical support, from what I’ve read elsewhere they seem to have a reputation of being very responsive as well as being happy to interact with the MediaPortal development team. They also make their SDK (software development kit) freely available.