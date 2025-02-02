---
title: 'Elitedesk 800 G6 Mini BIOS Password Removal'
date: '2025-02-02T00:00:00+00:00'
permalink: /it/hp-elite-800-bios-password-removal/
author_profile: true
excerpt: 'Remove the BIOS password from an HP Elite 800 G6 without EPROM programer'
layout: single
categories:
    - it
tags:
    - 'hp'
    - 'bios password'
    - 'eprom'
    - 'elitedesk'
---
Kia ora folks,
I bought this machine 2nd hand off Trademe and it sat for several months before I got around to using it, at which point I discovered it had a BIOS password. I queried the seller but didn’t get a reply.

[![](/content/uploads/2025/02/02/4.elite800g6.png){: width="450" }](/content/uploads/2025/02/02/4.elite800g6.png)

It is [well known](https://www.badcaps.net/forum/troubleshooting-hardware-devices-and-electronics-theory/troubleshooting-desktop-motherboards-graphics-cards-and-pc-peripherals/bios-schematic-requests/99026-hp-elitedesk-805-g6-mini-pc) that the password can be removed by dumping, modifying and re-flashing the BIOS chip with an external EPROM programmer, but then I found this tweet, so full credit for this one goes to [@anotheryeenbean](https://x.com/anotheryeenbean/status/1757285531644256428).

[![](/content/uploads/2025/02/02/0.anotheryeenbean.png){: width="450" }](/content/uploads/2025/02/02/0.anotheryeenbean.png)


I reached out to Alice for more info but didn't hear anything back and was too impatient, so decided to risk it. It seems to have worked so I'm documenting it more fully here. Use at your own risk of course! Not sure if it will work on the small-form-factor or tower models. Those may have a password reset jumper anyway? 

Not sure if it will cause any issues further down the track, but from what I can tell everything is intact in terms of IDs and Windows activation was not impacted. 

Also worth noting when doing anything with these HP Elitedesk machines, after full power loss, hard reset, and various other things, the machine will sit for ages with a black screen, powering itself on and off a few times doing self-tests etc. Just wait. Give it at least 3 mins before you decide it is failing to boot. 


Here’s what I did. 

1. I disconnected the power supply, got a jumper wire in place, holding one side on a ground point and the other on pin 2 of the GigaDevice flash chip. I believe this is the 16MB flash chip used by the sure start embedded controller, it’s not the main BIOS chip. 

[![](/content/uploads/2025/02/02/1.BIOS-chip.png){: width="450" }](/content/uploads/2025/02/02/1.BIOS-chip.png)

[![](/content/uploads/2025/02/02/2.ground-pin-2.png){: width="450" }](/content/uploads/2025/02/02/2.ground-pin-2.png)

{:start="2"}
2. Reconnected the power supply and attempted to power on the machine while holding the jumper wire in place. The power light flashed red and white (I didn’t get the exact sequence). 
3. Removed the jumper and powered off the machine.
4. It now fails to power on at all 😱. Disconnect the power supply and tried to power on to drain caps. 
5. Reconnect power supply and power on. Enter BIOS and everything is reset to factory. Good to go!

[![](/content/uploads/2025/02/02/3.BIOSScreen.png){: width="450" }](/content/uploads/2025/02/02/3.BIOSScreen.png)

All set for the new mini-cluster build!

[![](/content/uploads/2025/02/02/5.cluster-ready.png){: width="450" }](/content/uploads/2025/02/02/5.cluster-ready.png)


Hope this helps someone, and big thanks to Alice, no idea how you discovered this!
