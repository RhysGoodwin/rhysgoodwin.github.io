---
id: 154
title: 'Ipaq Auto-wake for in-car Applications'
date: '2009-03-21T23:31:09+13:00'
author: RhysGoodwin
excerpt: 'An easy to build DC/DC 5 Volt power supply. (Version 2) Great for car based projects. GPS , Ipaq, Car PC.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=154'
permalink: /electronics/ipaq-auto-wake-for-in-car-applications/
categories:
    - Electronics
tags:
    - IpAQ
    - 'ipaq car'
    - 'ipaq gps'
    - 'ipaq wake'
---

![update](/content/uploads/2009/04/update.jpg?w=300 "update")[As described a couple of posts ago](https://blog.rhysgoodwin.com/2008/11/26/dcdc-5v-power-supply-for-in-car-ppcgps/) I have set up an Ipaq 2210 in my car for the purpose of GPS navigation; it’s been working very well for a few months now but something stated to bug me; every time I got in the car I had to manually power on the Ipaq. I know it seems trivial and lazy but I just think such a device should be considered part of the car’s instrumentation, it should just sit there doing its thing; you wouldn’t want to have to manually turn on your speedo, rev counter or heat gauge would you?

After much digging I found that the Ipaq (the 2210 at least) could be woken up by applying 5volts to the DCD pin (pin 6) on the base connector; supposedly through a 4k7 resistor although that didn’t work for me so I just fed the 5v straight in (so far so good!). Since my power supply only applies power to the Ipaq at ignition I was able to just take a wire from the power-in on the Ipaq connector across to pin 6 (DCD). Now at ignition the Ipaq gets its 5volts for charging and the DCD gets 5volts to wake it up. There is just one problem; well two actually! The first is simple to fix; when the Ipaq is woken by DCD it tries to sync so the GPS application loses focus. There is an option somewhere in control panel to prevent this behaviour. The second problem is not quite so straight forward. When you turn the key in the ignition you get power for a second (DCD wake-up is triggered) then power is cut for a few seconds while the engine is cranking. Apparently the Ipaq won’t wake up in these conditions! So for this to work you need to turn the car on, wait for the Ipaq to wake up *then* start the car. Hardly seamless!

In the end I decided to rebuild the power supply with a 10 second delay on the output to the Ipaq. Thanks to [Bill Bowden for his Power-On Time Delay Relay schematic](http://ourworld.compuserve.com/homepages/Bill_Bowden/page2.htm#delay.gif).

If you want any more details or the PCB design in either PDF or eagle just let me know via a comment.

The finished project was much tidier than the [first build](/content/uploads/2009/04/done.jpg) but still hardly a professional finish! It does however work perfectly and the GPS requires no user intervention!

[![Power Supply built into small project box](/content/uploads/2009/04/version2-1.jpg?w=300 "Power Supply built into small project box")](/content/uploads/2009/04/version2-1.jpg)

[![version2-2](/content/uploads/2009/04/version2-2.jpg?w=300 "Power Supply built into small project box ")](/content/uploads/2009/04/version2-2.jpg)

[![pcb1](/content/uploads/2009/04/pcb1.jpg "pcb1")](/content/uploads/2009/04/pcb1.jpg)