---
id: 665
title: 'Vmware Player Unity (Trackpad Scrolling Works!)'
date: '2010-10-30T10:53:22+13:00'

excerpt: 'VMWare Player Unity and trackpad scrolling within guest VM.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=665'
permalink: /windows-desktop/vmware-player-unity-trackpad-scrolling-works/
categories:
    - 'Windows Desktop'
tags:
    - player
    - scrolling
    - synaptics
    - touchpad
    - trackpad
    - unity
    - vmware
---

I like to use VMwaer Player to compartmentalise different tasks on my laptop. For example I have a Visual Studio development virtual machine. It’s got all the SDKs and tools I need for development. This has a couple of advantages:

1. It keeps things tidy, I don’t have to have all sorts of SDKs and tools on my local OS, at the same time my development environment is also kept clean
2. The development environment is very portable. If I want to rebuild my laptop I can just backup the entire VM and I don’t have to worry about setting it up all over again

This only issue I’ve had is that I’ve never been able to get trackpad scrolling to work within the virtual machine. This makes VS development a bit painful!

One of the new features of VMWare Player 2.x is “Unity”, it allows you use apps within the guest VM as if they were running on your local OS. Once you enter Unity mode you will see the Virtual Machine window minimnise and all the apps that you had running inside the VM appear in your local taskbar. It’s a bit like the way Citrix seamless window app publishing works. In Unity trackpad scrolling works!

[![](/content/uploads/2010/10/VMWarePlayerUnity.jpg "VMWare Player Unity")](/content/uploads/2010/10/VMWarePlayerUnity.jpg)

Now you get the best of all worlds – compartmentalisation of environments with a native OS feel **and** trackpad scrolling!