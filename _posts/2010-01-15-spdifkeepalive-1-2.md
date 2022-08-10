---
id: 326
title: 'SPDIFKeepAlive 1.2'
date: '2010-01-15T11:20:53+13:00'

excerpt: 'SPDIFKeepAlive Updated! New Version 1.2!'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=326'
permalink: /htpc/spdifkeepalive-1-2/
categories:
    - HTPC
    - 'Windows Desktop'
tags:
    - Keepalive
    - 'sony unlock'
    - SPDIF
    - 'spdif delay'
---

I finally got around to fixing the auto play on start-up problem. The reason it took so long was because I was trying to perfect the auto restart option:

Sometimes when an AC3 file is played or something that takes control of the sound card SPDIFkeepAlive gets stopped. The auto restart option restarts playback every 3 seconds. This means that shortly after AC3 playback stops SPDIFKeepAlive resumes automatically.

Unfortunately I’ve still not got it working properly, at least not for me, depending on your sound card / driver you might get better results. But be warned, on my system after a random amount of time (~1 hour) I get a terrible noise produced by SpdifKeepAlive and I have to exit, hence I don’t use this feature! I’ll try to fix it sometime. I think I’ll need to use threading and play 2 files that continuously overlap.

You can download the new version at the bottom of the [SPDIFKeepAlive Post](https://blog.rhysgoodwin.com/windows-desktop/spdif-keepalive/)