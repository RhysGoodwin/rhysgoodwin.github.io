---
id: 582
title: 'Samsung LCD Stuck on Australia after Firmware Upgrade'
date: '2010-10-01T18:50:43+13:00'

excerpt: 'Samsung LCD country setting is locked to the wrong country.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=582'
permalink: /home-theatre/samsung-stuck-on-australia-after-firmware-upgrade/
categories:
    - 'Home Theatre'
tags:
    - B650
    - 'country code'
    - LCD
    - samsung
    - 'series 6'
---

Against my better judgement and for no other reason than the fact that I wasn’t running the latest version I decided to upgrade the firmware on my Samsung series 6 LCD (LA40B650) . Version 1007 to version 2002. Now you’d think that a firmware downloaded from the New Zealand Samsung site would be suitable for a New Zealand TV, but after upgrading, the country was set to Australia and was locked there. Now the problem with that is that in Aussie they use 7Mhz for DVB-T whereas we use 8Mhz.

[![](/content/uploads/2010/10/Samsung.jpg "Samsung")](/content/uploads/2010/10/Samsung.jpg)

This is how I got it fixed. (**<span style="color: #ff0000;">\*Use at your own risk\*</span>**)

<span style="color: #ff0000;">\*EDIT\*</span>

<span style="color: #ff0000;"> </span>If you have this problem, make sure you’re on the tuner input when you try to change the country in the settings. It seems like my whole problem might have been because I was on the HDMI1 input while trying to change it!

<span style="color: #ff0000;">\*EDIT\*</span>

I went in to the service menu.

**INFO**, **MENU**, **MUTE**, **POWER**

The TV comes on with the service menu displayed. I did a factory reset. When the TV came on, it displayed the setup wizard, at which point you can select the country – unfortunately New Zealand isn’t an option. I selected “**others**“. I then went back into the service menu and changed the region from Asia\_ATV to Asia\_**D**TV. The TV is now locked to Australia again! But now I can change the country code by entering a code. I just entered 000. It then let me choose Australia, New Zealand or Singapore. Done!

On a side note: I’ve tried for several hours to get RS-232 control (i.e. power on/off change input etc) working, but to no avail. I know the serial cable is working because when I put the RS-232 port into debug mode (via service menu) I get plenty of debug messages in the terminal window. If anyone else has had success please let me know via comment.