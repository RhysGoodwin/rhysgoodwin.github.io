---
id: 2277
title: 'Brasilia Espresso Machine Restoration and PID Upgrade'
date: '2015-07-04T19:29:57+13:00'
author: RhysGoodwin
excerpt: 'Brasilia ''Lady'' coffee machine restoration and modification with oLED display and Arduino PID.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=2277'
permalink: /fabrication/brasilia-espresso-machine-restoration-and-pid-upgrade/
embed:
    - ''
url:
    - ''
lightbox-url:
    - ''
testimonial:
    - ''
testimonial_author:
    - ''
image: /wp-content/uploads/2015/07/Assembly13.jpg
categories:
    - Fabrication
tags:
    - arduino
    - brasilia
    - hack
    - lady
    - lathe
    - milling
    - PID
---

I’ve been working on this project on and off for a few years. It started off as a simple restoration of a second hand Italian espresso machine which quickly got out of control, as most of my projects seem to do. Here’s a video showing the finished project and then a bunch of photos showing the build. I should have done the video with the camera turned the other way, sorry about that but I couldn’t be bothered re-doing it.

<iframe allowfullscreen="true" class="youtube-player" height="390" src="https://www.youtube.com/embed/OJqH6CJR3TU?version=3&rel=1&fs=1&autohide=2&showsearch=0&showinfo=1&iv_load_policy=1&wmode=transparent" style="border:0;" type="text/html" width="640"></iframe>

The Brasilia ‘Lady’ is a very simple single-group, single boiler machine. It has a 300ml brass boiler with a 3-way solenoid valve. It has a simple bimetallic thermostat which means the temperature swings wildly (although some models do have more complex thermostats). My model had no micro controller and was purely AC driven and controlled by the buttons on the front and the thermostat. 

When I started restoring the machine I quickly decided that I wanted to do a PID modification to maintain a constant temperature. At the time I had just started playing around with Arduino so I thought why not just take all of the AC buttons on the front down to an Arduino and control everything through software with solid state relays for the pump, boiler and solenoid. The pictures and captions below should explain each part of the build sufficiently.

TLDR: Final assembly photos are at the bottom of the post.

### Machine Housing

[![Case01](/content/uploads/2015/07/Case01.jpg)](/content/uploads/2015/07/Case01.jpg)This is how the machine started out. This isn’t actually mine but I didn’t take a photo before I started. Mine was in much worse condition.

[![Very simplistic internals.](/content/uploads/2015/07/case00.jpg)](/content/uploads/2015/07/case00.jpg)Very simplistic internals.

[![Case02](/content/uploads/2015/07/Case02.jpg)](/content/uploads/2015/07/Case02.jpg)

[![Case03](/content/uploads/2015/07/Case03.jpg)](/content/uploads/2015/07/Case03.jpg)

[![case04](/content/uploads/2015/07/case04.jpg)](/content/uploads/2015/07/case04.jpg)



### Top Panel

[![TopPanel01](/content/uploads/2015/07/TopPanel01.jpg)](/content/uploads/2015/07/TopPanel01.jpg)Template for top panel created in SketchUp

[![Template printed onto toner transfer paper](/content/uploads/2015/07/TopPanel02.jpg)](/content/uploads/2015/07/TopPanel02.jpg)Template printed onto toner transfer paper

Thanks to Katt for the great idea of using [PCB transfer paper.

[![TopPanel03](/content/uploads/2015/07/TopPanel03.jpg)](/content/uploads/2015/07/TopPanel03.jpg)Template pressed onto 3.5MM aluminium plate with an old sandwich press

[![TopPanel04](/content/uploads/2015/07/TopPanel04.jpg)](/content/uploads/2015/07/TopPanel04.jpg)

[![TopPanel05](/content/uploads/2015/07/TopPanel05.jpg)](/content/uploads/2015/07/TopPanel05.jpg)

[![TopPanel06](/content/uploads/2015/07/TopPanel06.jpg)](/content/uploads/2015/07/TopPanel06.jpg)Milling the display aperture and a recess so that the display is very close to the surface of the panel.

This Sieg SX3 mill is proving to be very useful. It’s the same as the [Grizzly G0619](http://www.amazon.com/gp/product/B000M66WKO/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B000M66WKO&linkCode=as2&tag=blogrhysgoodw-20&linkId=TKR5NWA77KKILAS6)![](https://ir-na.amazon-adsystem.com/e/ir?t=blogrhysgoodw-20&l=as2&o=1&a=B000M66WKO)

[![TopPanel07](/content/uploads/2015/07/TopPanel07.jpg)](/content/uploads/2015/07/TopPanel07.jpg)

[![TopPanel08](/content/uploads/2015/07/TopPanel08.jpg)](/content/uploads/2015/07/TopPanel08.jpg)

[![TopPanel09](/content/uploads/2015/07/TopPanel09.jpg)](/content/uploads/2015/07/TopPanel09.jpg)

[![TopPanel10](/content/uploads/2015/07/TopPanel10.jpg)](/content/uploads/2015/07/TopPanel10.jpg)Polishing compounds in various grades

The polishing compounds came in a [set of 12 syringes](http://www.amazon.com/gp/product/B00N5HAPV4/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00N5HAPV4&linkCode=as2&tag=blogrhysgoodw-20&linkId=IJHEYMY3NWSSCEBY)![](https://ir-na.amazon-adsystem.com/e/ir?t=blogrhysgoodw-20&l=as2&o=1&a=B00N5HAPV4)

[![TopPanel11](/content/uploads/2015/07/TopPanel11.jpg)](/content/uploads/2015/07/TopPanel11.jpg)Shiny!

### oLED Display Module

[![Display02](/content/uploads/2015/07/Display02.jpg)](/content/uploads/2015/07/Display02.jpg)

[![Display03](/content/uploads/2015/07/Display03.jpg)](/content/uploads/2015/07/Display03.jpg)

[![Display04](/content/uploads/2015/07/Display04.jpg)](/content/uploads/2015/07/Display04.jpg)

[![Display05](/content/uploads/2015/07/Display05.jpg)](/content/uploads/2015/07/Display05.jpg)

[![Display06](/content/uploads/2015/07/Display06.jpg)](/content/uploads/2015/07/Display06.jpg)

[![Display07](/content/uploads/2015/07/Display07.jpg)](/content/uploads/2015/07/Display07.jpg)

[![Display08](/content/uploads/2015/07/Display08.jpg)](/content/uploads/2015/07/Display08.jpg)

[![Display09](/content/uploads/2015/07/Display09.jpg)](/content/uploads/2015/07/Display09.jpg)

[  
![Display95](/content/uploads/2015/07/Display95.jpg)](/content/uploads/2015/07/Display95.jpg)

This [3M Double-sided tape](http://www.amazon.com/gp/product/B00LWFO5R6/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00LWFO5R6&linkCode=as2&tag=blogrhysgoodw-20&linkId=UJXILHYO63TSNWXB)![](https://ir-na.amazon-adsystem.com/e/ir?t=blogrhysgoodw-20&l=as2&o=1&a=B00LWFO5R6) is super strong.

[![Display10](/content/uploads/2015/07/Display10.jpg)](/content/uploads/2015/07/Display10.jpg)

[![Display11](/content/uploads/2015/07/Display11.jpg)](/content/uploads/2015/07/Display11.jpg)The clear acrylic lens is sealed to the display module with 3M double sided tape.

[![Display12](/content/uploads/2015/07/Display12.jpg)](/content/uploads/2015/07/Display12.jpg)

[![Display13](/content/uploads/2015/07/Display13.jpg)](/content/uploads/2015/07/Display13.jpg)

### PCB and Controller

[![PCB design created in KICAD. Such an awesome piece of software!](/content/uploads/2015/07/PCBKiCad.png)](/content/uploads/2015/07/PCBKiCad.png)PCB design created in KICAD. Such an awesome piece of software!

[![Controller01](/content/uploads/2015/07/Controller01.jpg)](/content/uploads/2015/07/Controller01.jpg)12 double layered PCBs delivered for $14 USD from DirtyPCBs.com. I’m not complaining!

[![Controller02](/content/uploads/2015/07/Controller02.jpg)](/content/uploads/2015/07/Controller02.jpg)

[![Controller03](/content/uploads/2015/07/Controller03.jpg)](/content/uploads/2015/07/Controller03.jpg)

[![Controller04](/content/uploads/2015/07/Controller04.jpg)](/content/uploads/2015/07/Controller04.jpg)

[![Controller05](/content/uploads/2015/07/Controller05.jpg)](/content/uploads/2015/07/Controller05.jpg)

[![Controller06](/content/uploads/2015/07/Controller06.jpg)](/content/uploads/2015/07/Controller06.jpg)

[![Controller07](/content/uploads/2015/07/Controller07.jpg)](/content/uploads/2015/07/Controller07.jpg)

### Power Box

[![PowerBox01](/content/uploads/2015/07/PowerBox01.jpg)](/content/uploads/2015/07/PowerBox01.jpg)

### Front LED

[![Led01](/content/uploads/2015/07/Led01.jpg)](/content/uploads/2015/07/Led01.jpg)

[![Led02](/content/uploads/2015/07/Led02.jpg)](/content/uploads/2015/07/Led02.jpg)The led holder is made from stainless steel. A piece of fiber optic plastic is glued in as a lens.

### Water Tank

[![Tank01](/content/uploads/2015/07/Tank01.jpg)](/content/uploads/2015/07/Tank01.jpg)

[![Tank02](/content/uploads/2015/07/Tank02.jpg)](/content/uploads/2015/07/Tank02.jpg)

[![Tank03](/content/uploads/2015/07/Tank03.jpg)](/content/uploads/2015/07/Tank03.jpg)

[![Tank04](/content/uploads/2015/07/Tank04.jpg)](/content/uploads/2015/07/Tank04.jpg)

[![Tank05](/content/uploads/2015/07/Tank05.jpg)](/content/uploads/2015/07/Tank05.jpg)

[![Tank06](/content/uploads/2015/07/Tank06.jpg)](/content/uploads/2015/07/Tank06.jpg)

[![Tank07](/content/uploads/2015/07/Tank07.jpg)](/content/uploads/2015/07/Tank07.jpg)

### Water Inlet

[![WaterInlet01](/content/uploads/2015/07/WaterInlet01.jpg)](/content/uploads/2015/07/WaterInlet01.jpg)

[![WaterInlet02](/content/uploads/2015/07/WaterInlet02.jpg)](/content/uploads/2015/07/WaterInlet02.jpg)

[![WaterInlet03](/content/uploads/2015/07/WaterInlet03.jpg)](/content/uploads/2015/07/WaterInlet03.jpg)

[![WaterInlet04](/content/uploads/2015/07/WaterInlet04.jpg)](/content/uploads/2015/07/WaterInlet04.jpg)

[![WaterInlet05](/content/uploads/2015/07/WaterInlet05.jpg)](/content/uploads/2015/07/WaterInlet05.jpg)

[![WaterInlet06](/content/uploads/2015/07/WaterInlet06.jpg)](/content/uploads/2015/07/WaterInlet06.jpg)

I’m not sure why I made such an elaborate nut for this considering it’s hidden inside the machine. Never mind it was fun.

[![WaterInlet07](/content/uploads/2015/07/WaterInlet07.jpg)](/content/uploads/2015/07/WaterInlet07.jpg)

[![WaterInlet08](/content/uploads/2015/07/WaterInlet08.jpg)](/content/uploads/2015/07/WaterInlet08.jpg)

### Final Assembly

Before the final assembly I had the machine casing stripped and powder coated in flame red.

[![Assembly01](/content/uploads/2015/07/Assembly01.jpg)](/content/uploads/2015/07/Assembly01.jpg)Group head temperature probe

The group head temperature probe is held on with [High temperature Kapton tape](http://www.amazon.com/gp/product/B0049KTI8W/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B0049KTI8W&linkCode=as2&tag=blogrhysgoodw-20&linkId=6NQUBFXV7V2QNUOW)![](https://ir-na.amazon-adsystem.com/e/ir?t=blogrhysgoodw-20&l=as2&o=1&a=B0049KTI8W)

[![Assembly02](/content/uploads/2015/07/Assembly02.jpg)](/content/uploads/2015/07/Assembly02.jpg)

[![Assembly03](/content/uploads/2015/07/Assembly03.jpg)](/content/uploads/2015/07/Assembly03.jpg)

[![Assembly04](/content/uploads/2015/07/Assembly04.jpg)](/content/uploads/2015/07/Assembly04.jpg)

[![Assembly05](/content/uploads/2015/07/Assembly05.jpg)](/content/uploads/2015/07/Assembly05.jpg)The tube wrapped around the boiler is a pre-heat tube that I added so that the water being drawn into the boiler is not stone cold.

[![Assembly06](/content/uploads/2015/07/Assembly06.jpg)](/content/uploads/2015/07/Assembly06.jpg)

[![Assembly07](/content/uploads/2015/07/Assembly07.jpg)](/content/uploads/2015/07/Assembly07.jpg)

[![Assembly08](/content/uploads/2015/07/Assembly08.jpg)](/content/uploads/2015/07/Assembly08.jpg)

[![Assembly09](/content/uploads/2015/07/Assembly09.jpg)](/content/uploads/2015/07/Assembly09.jpg)

[![Assembly10](/content/uploads/2015/07/Assembly10.jpg)](/content/uploads/2015/07/Assembly10.jpg)

[![Assembly11](/content/uploads/2015/07/Assembly11.jpg)](/content/uploads/2015/07/Assembly11.jpg)

[![Assembly12](/content/uploads/2015/07/Assembly12.jpg)](/content/uploads/2015/07/Assembly12.jpg)

[![Assembly13](/content/uploads/2015/07/Assembly13.jpg)](/content/uploads/2015/07/Assembly13.jpg)

[![Final](/content/uploads/2015/07/Final.jpg)](/content/uploads/2015/07/Final.jpg)

#### Parts List: (Thanks China!)

- Top plate: 3.5mm Aluminium (from HP Server blanking panels)
- Display module: Cut from a block for 101 x 50mm Aluminium (New, local) 
    - Display: Hide.HK I2C 1602 LED display. (via eBay ~20USD)
    - Display glass: Plexiglass 1.5mm (via ebay)
    - Display Tape: 3M 300LSE 9495LE Double Sided Adhesive (sheets via AliExpress)
- PCB printed by DirtyPCBs.com 
    - Arduino Clone: Nano 3.0 clone (via AliExpress)
    - 2x Temperature sensor chip: MAX6675ISA SPI Interface (via AliExpress)
    - Connectors (via local JayCar Electronics)
- PCB Box (via AliExpress)
- Power Box – Aluminium (via local JayCar Electronics) 
    - Solid-State relays (Can’t remember had bunch in a parts box for years)
    - DC power supply: From a Samsung USB charger
    - Rubber grommet kit (via AliExpress)
- LED holder: From some stainless rod I had lying around
- Water Take outlet: Stainless M10 Bolt 
    - Silicon seal for water inlet – from a kit (via AliExpress)
- Water inlet: 22mm Aluminum rod (New, local)
- Silicon tube: 6x9mm food grade (via AliExpress)
- Pre-Heat tube: 6.4mm (I think) copper (New, Local)

Thanks for stopping by. Feel free to ask questions.

Here is the source code and PCB schematic designs if anyone is interested. I’d be happy to have critique on either.

[ uCespresso PCB (1382 downloads) ](https://blog.rhysgoodwin.com/download/2471/)[ uCespresso Arduino Code (1532 downloads) ](https://blog.rhysgoodwin.com/download/2473/)