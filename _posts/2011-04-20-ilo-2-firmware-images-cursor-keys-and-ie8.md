---
id: 1301
title: 'iLO 2 Firmware Images, Cursor Keys and IE8'
date: '2011-04-20T19:45:26+13:00'

excerpt: 'Where to find .bin firmware images file for HP iLO2 and a workaround for cursor keys not working in iLO remote console.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1301'
permalink: /windows-admin/ilo-2-firmware-images-cursor-keys-and-ie8/
categories:
    - 'Windows Admin'
tags:
    - .bin
    - 'arrow keys'
    - 'cursor keys'
    - firmware
    - HP
    - ie8
    - ilo
    - image
    - 'protected mode'
---

A couple of things I came accross with iLO today:

[![](/content/uploads/2011/04/ILOFlash.jpg "ILOFlash")](/content/uploads/2011/04/ILOFlash.jpg)

**Plain Firmware Image .bin Files**

I’ve got DL360 G5 running VMWare ESX 4 and I wanted to update the iLO firmware to the latest version. Even though iLO has a firmware update page where you can upload a new firmware image file. This doesn’t seem to be available for download at the HP iLO2 support page. To get it you need to download the Windows firmware update tool and extract the package using 7-zip.

**Remote Console – Cursor Keys Don’t Work with IE8**

To get around this, disable protected mode in IE or run it IE as administrator (Windows 7, vista etc).