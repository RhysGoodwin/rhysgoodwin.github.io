---
id: 607
title: '$10 USB DVB-T Receiver Review'
date: '2010-10-22T21:07:19+13:00'
author: RhysGoodwin
excerpt: 'Mini review of a very cheap USB DVB-T dongle'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=607'
permalink: /htpc/10-usb-dvb-t-receiver-review/
embed:
    - ''
categories:
    - HTPC
tags:
    - AF9015
    - cheap
    - dongle
    - DVB-T
    - ebay
    - freeview
    - HD
    - HTPC
    - mediaportal
    - USB
---

After about 2½ years of constant use my trusty Hauppauge HVR-2200 dual DVB-T tuner card started to fail. Just for fun I bought a dirt cheap USB receiver off eBay![](https://rover.ebay.com/roverimp/1/710-53481-19255-0/1?ff3=2&pub=5574939060&toolid=10001&campid=5336746271&customid=1002&item=150460308676&mpt=[CACHEBUSTER]).

[![](/content/uploads/2010/10/USBDVB-TPackage.jpg "USB DVB-T Package")](/content/uploads/2010/10/USBDVB-TPackage.jpg)

Total outlay was $15.09 NZD shipped. That’s $10.33 for everything you see above except the MCX to ‘F’ adaptor which was $4.77. It took almost 3 weeks to arrive from China. Given the price, I wasn’t really expecting much, but it was so cheap I thought it was worth getting it just to play around with.

[![](/content/uploads/2010/10/Generic-AF9015-USB-DVB-T.jpg "Generic AF9015 USB DVB-T")](/content/uploads/2010/10/Generic-AF9015-USB-DVB-T.jpg)

Apparently there is a variety of different chipsets used in these devices, all of which come wrapped in the same outer casing. One difference between the various types is the LED, on some of them it’s red, other’s its clear. From what I’ve gathered these are the possible chipsets:

- Intel CE6230 (Intel CE9500 reference design)
- e3C EC168
- Afatech AF9015

[![](/content/uploads/2010/10/AF9015A1.jpg "AF9015A")](/content/uploads/2010/10/AF9015A1.jpg)

I got the one with the Afatech AF9015. I didn’t want to plug it straight into my nice, clean, recently rebuilt HTPC server in case the drivers got all ‘tangled up’. Instead, I plugged it into my laptop running Windows 7 Pro x64. Windows immediately detected the device as a **Leadtek WinFast DTV Dongle Gold**. I was happy to see this, because I had no intention of using the bundled driver mini-CD – who knows how old and buggy that driver is!

The device installed cleanly without any issues, so I decided to to plug it into my HTPC server which is running Windows 7 Ultimate x64. Of course I expected the device to install automatically, as it had on my laptop, but it didn’t, despite using the search online option. Fortunately I now knew that the Leadtek driver was compatible, so I went to the Leadtek site and downloaded the driver. As always, I extracted the setup package using 7-zip which gave me the plain driver folder as opposed to running the setup, which will install who knows what!

Windows found the driver and installed it correctly!

[![](/content/uploads/2010/10/DVB-TDeviceManager.jpg "Windows 7 Device Manager")](/content/uploads/2010/10/DVB-TDeviceManager.jpg)

After restarting the MediaPortal TV Server service, it detected the device and I set it to highest priority so I could put it though its paces. The first time it tunes a channel after a reboot it takes approximately 30 seconds, but subsequent channel changes are quite fast ~3 seconds.

[![](/content/uploads/2010/10/TVServer.jpg "MediaPortal TV Server")](/content/uploads/2010/10/TVServer.jpg)

I’m pleased (and surprised) to report that this cheap little tuner has been working flawlessly for an entire week. How long it will last remains to be seen! For $10 I can definitely recommend this device – the only issue is that you might get a totally different chipset; that is, a totally different device for which none of the above would apply. If you purchase from the seller I linked to at the beginning of the post there’s a good chance you’ll get the same device.

The included antenna and remote are complete rubbish and will quickly find their way to the bin!