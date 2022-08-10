---
id: 29
title: 'SPDIF KeepAlive for Home Theatre PC'
date: '2008-05-10T15:20:34+13:00'

excerpt: 'Free SPDIF Keep-alive utility. Are you getting a delay when you use SPDIF from your PC to your receiver or Unlock message on your Sony receiver? Here is the fix. Free Download!'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=29'
permalink: /htpc/spdif-keepalive/
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
categories:
    - HTPC
    - 'Windows Desktop'
tags:
    - Keepalive
    - 'sony unlock'
    - SPDIF
    - 'spdif delay'
---

![Optical SPDIF Connector](/content/uploads/2009/03/spdifconnector.jpg "Optical SPDIF Connector")

- - - - - -

**<span style="color: #ff0000;">\*\*Update\*\*</span>**

I suggest you look at Veg’s S[oundKeeper tool](http://veg.by/en/projects/soundkeeper/) first and see if it does what you need. It looks like a much cleaner and more efficient tool than mine (which is now nearly 10 years old! :o). Nice work Veg.

- - - - - -

After building a new Home Theatre PC I’ve discovered that the onboard IDT audio has a problem with the SPDIF output, or at least my Sony receiver has a problem with it! Every time a sound is played it causes the SPDIF input on the receiver to initialise which takes about 500 milliseconds, after the sound has finished the SPDIF goes back to sleep. As a result the first 500ms is lost off every sound that is played; not really a problem if you’re watching a movie but for applications that have little blips as you navigate around these sounds tend to get missed altogether; such is the case in MediaPortal the HTPC application I use.

My old motherboard with Nforce sound didn’t have this problem the SPDIF remained “active” all the time.

After much searching I did find a few other people with the same problem but no solution so I’ve written a small .NET application called SPDIFKeepAlive. It does just that. It sits in the system tray and continuously plays a silent wave file to keep SPDIF port active.

[![SPDIFKeepAlive Settings](/content/uploads/2008/05/interface.jpg "SPDIFKeepAlive Settings")](/content/uploads/2008/05/interface.jpg)

Hope this is helpful for others, leave comments for bugs / feature requests etc

### Version History

Version 1.0

- Initial release

Version 1.1

- Added new output options

Version 1.2 (Current Version)

- Fixed auto play on startup
- Added auto-restart option

### Downloads 

[ SPDIFKeepAlive-1.2.exe ](/content/uploads/downloads/2017/03/SPDIFKeepAlive-1.2.exe)(.Net 2.0 Required)

Source Code – Feel free to do what you like with this (no license)  
[ SPDIFKeepAlive-Source-v1.2.zip](/content/uploads/downloads/2017/03/SPDIFKeepAlive-Source-v1.2.zip)