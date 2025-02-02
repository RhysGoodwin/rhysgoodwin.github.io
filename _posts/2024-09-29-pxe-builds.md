---
title: 'PXE booted Linux installs with secure boot'
date: '2024-09-29T00:00:00+00:00'
permalink: /it/pxe-builds/
author_profile: true
excerpt: 'Notes for my PXE host building framework'
layout: single
categories:
    - it
tags:
    - 'signed kernel'
    - 'uefi'
    - 'kickstart'
    - 'ubuntu autoinstall'
    - 'pxe'
    - 'rocky'
    - 'secure boot'
    - 'mokutil'
---

# Get Serial for Ubuntu autoinstall
udevadm info --query=all --name=/dev/sdX | grep ID_SERIAL

# Add certificate to UEFI 
```bash
wget http://mgmt.home.rhysgoodwin.com/ca.i.rhysmg.nz.der
mokutil --import ca.i.rhysmg.nz.der
```
Then reboot and enter the password. 

# Order
 - pxe
 - base
    - 
