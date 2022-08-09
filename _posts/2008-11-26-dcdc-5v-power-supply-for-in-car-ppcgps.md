---
id: 125
title: 'DC/DC 5V Power Supply for in car PPC/GPS'
date: '2008-11-26T23:15:23+13:00'
author: RhysGoodwin
excerpt: 'An easy to build DC/DC 5 Volt power supply. Great for car based projects. GPS , Ipaq, Car PC.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=125'
permalink: /electronics/dcdc-5v-power-supply-for-in-car-ppcgps/
categories:
    - Electronics
tags:
    - '5 volt'
    - 'car computer'
    - 'car powersupply'
    - GPS
    - IpAQ
    - LT1074
    - PPC
---

[<span style="color: #ff0000;">Click here if your looking for the updated version of the DC/DC 5 volt power supply</span>](https://blog.rhysgoodwin.com/electronics/ipaq-auto-wake-for-in-car-applications/)

I had a serial GPS mouse lying around (Thanks Alex) and my boss was kind enough to give me a retired HP Ipaq 2210 pocket PC from work. The 2 combined and I had a pretty reasonable touch screen car navigation system. Only problem was a power supply.

I didn’t want to use a cigarette lighter adaptor because I would have wires going everywhere; I wanted it hard-wired so the the GPS mouse sits on the dash with the cable disappearing down behind and a single thin cable coming out from the centre console for the Ipaq. I also wanted the GPS mouse running full time so there was no delay when it was searching for the satellites but I only wanted the IPAQ to be powered when the ignition was on.

I made 3 attempts before I was successful. First was 2 LM7805 5volt regulators. These got way too hot even with a heatsink; I would have needed a huge heatsink. Second attempt was the contents of 2 cigarette lighter to USB adaptors supposedly able to deliver 1AMP; yeah right! These things just about burst in to flames when I turned on the IPAQ! The third and successful attempt uses a LT1074 switching regulator and is detailed below.

The LT1074 was provided as a sample from Linear Technology, which is great since they cost about $NZ40 to order from RS!

The schematic is just the reference one from the LT1074 datasheet. [![schematic](/content/uploads/2009/04/schematic.jpg "schematic")](/content/uploads/2009/04/schematic.jpg)

I couldn’t find exact matches for all the components in the reference schematic.

<span style="text-decoration: underline;">Here’s a list of parts I used</span> **C1:** Electrolytic 470uF (25v)  
**C2:** Green Cap 0.01uF (This was a guess! All I really new is that it wasn’t an electrolytic because the schematic shows no polarity symbols!)  
**C3:** Low ESR electrolytic 220uF (25v) (The application noste AN35 said to use low ESR and place it very close to the the LT1074).  
**MBR6745:** This a SCHOTTKY-BARRIER RECTIFIER DIODE the recommended MBR745 is rated at 7.5Amps I used an ERC81-004 rated at 3Amps. Robbed from an old dot matrix printer PSU.  
**R1:**I couldn’t find 2.8K @1% so I used 2x 5.6K @1% in parallel; both 1/2watt.  
**R2:**2.2K @1%; 1/2 watt  
**R3:**2.7k @5%; 1/2 watt  
**L1:** This is of unknown value; robbed from an old dot matrix printer PSU. The application notes AN35 describe a rather humerus “alternate” method of selecting and inductor: (Click to read)

[![inductorselection](/content/uploads/2009/04/inductorselection.jpg "inductorselection")](/content/uploads/2009/04/inductorselection.jpg)

I haven’t done any PCB etching since high school so I thought I’d give it a go. I used Eagle CAD PCB design software which allows boards 100mmx80mm to be designed using their freeware version. Eagle is a bit clunky and counter-intuitive but once you get the hang of it, it’s really very good.

[![pcb](/content/uploads/2009/04/pcb.jpg "pcb")](/content/uploads/2009/04/pcb.jpg)

I followed the laser printer method for my etching as described in [this instructables article](http://www.instructables.com/id/DIY_Printed_circuit_board/).

1.Print the design to some shiny paper. (Thanks to ASB Bank!) Clean up the copper board with some 1200 sand paper  
[![Iron On Design](/content/uploads/2009/04/ironon2.jpg "Iron On Design")](/content/uploads/2009/04/ironon2.jpg)

2\. Iron on the design  
[![Iron On](/content/uploads/2009/04/ironon4.jpg "Iron On")](/content/uploads/2009/04/ironon4.jpg)

3.Soak the board for 10Mins in cold water  
[![Soak](/content/uploads/2009/04/sink.jpg "Soak")](/content/uploads/2009/04/sink.jpg)

4.Clean the paper off the board  
[![Clean Up](/content/uploads/2009/04/cleanup.jpg "Clean Up")](/content/uploads/2009/04/cleanup.jpg)

5 Make up some etchant with Hydrochloric Acid and Hydrogen peroxide (Be careful!)  
[![Etch Solution](/content/uploads/2009/04/mixup.jpg "Etch Solution")](/content/uploads/2009/04/mixup.jpg)

6.Etch the board  
[![Etching](/content/uploads/2009/04/etching.jpg "Etching")](/content/uploads/2009/04/etching.jpg)

7.Etching Complete  
[![Etched](/content/uploads/2009/04/etched.jpg "Etched")](/content/uploads/2009/04/etched.jpg)

8.Drill, holes, add components and solder  
[![Done](/content/uploads/2009/04/done.jpg "Done")](/content/uploads/2009/04/done.jpg)

9.Test. Wow! It works! It has two +5v outputs, one is always on for the GPS the other is switched on via the relay at ignition.  
[![Testing](/content/uploads/2009/04/testing.jpg "Testing")](/content/uploads/2009/04/testing.jpg)

That’s it! A bit of shame I don’t really understand how it works! It does the job though and with very little heat.