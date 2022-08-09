---
id: 111
title: 'Dual Cloned Windows Install with Linux GRUB'
date: '2008-07-05T23:01:24+13:00'
author: RhysGoodwin
excerpt: 'Boot multiple isolated copies of Windows off the same hard drive using the free Linux boot loader "Grub". (Linux not required!)'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=111'
permalink: /windows-desktop/dual-cloned-windows-install-with-grub/
categories:
    - 'Windows Desktop'
tags:
    - 'cloned dual boot'
    - 'dual boot'
    - grub
    - 'gurb without linux'
    - multiboot
    - 'windows multiboot'
---

Ok this might be old news but I had some trouble finding a simple straight forward guide.

I just got a new laptop at work and I wanted to run to two completely separate copies of Windows XP using GNU GRUB as the boot loader.

[![Grub Boot Screen](/content/uploads/2009/04/dunes.png "Grub Boot Screen")](/content/uploads/2009/04/dunes.png)

These are the steps I eventually followed:

1\. Completely clear the drive, all windows partitions, recovery partition etc.

2\. Run XP setup, create a new partition leaving enough room for the second copy of windows; install Windows as per usual.

3\. Install all drivers, updates and any software that you want to be on both copies of windows.

4\. In windows Disk management create a new NTFS partition in the remaining space on the disk, leaving enough space for GRUB bootloader, I left 100MB,

5\. Run Symantc ghost (or some other disk cloning tool) and clone partition 1 to partition 2. You will now have 2 identical copes of Windows on the same drive.

6\. After cloning **don’t** reboot into windows, instead boot from CD to you’re favourite Linux distro. I used Ubuntu 6.1 which boots live off CD as part of it’s install process; I’m sure you could use knoppix or whatever.

7\. Create a Linux partition in the remaining space using fdisk or cfdisk; flag this partition as the bootable partition. Assuming the disk is **/dev/hda**; XP1 and XP2 are **/dev/hda1** and **/dev/hda2** then this partition will be **/dev/hda3**

```
<pre style="text-align: left;">cfdisk /dev/hda
```

.

8\. Format the new Linux partition with

```
mkfs.ext3 /dev/hda3
```

.

9\. Make a new directory and mount the partition to. For this example e.g we’ll mount it to /mnt/tmp

```
mkdir /mnt/tmp
mount /dev/hda3 /mnt/tmp
```

.

10\. Install Grub to the partition. This Will install grub to the root of /dev/hda3 and to the master boot record of /dev/hda

```
grub-install --root-directory=/mnt/tmp /dev/hda
```

.

11\. Create a grub menu list file in in /mnt/tmp/boot/grub/  
Use vi or nano to create a file in this location called menu.lst This will contain a list of operating systems you wish to boot. The file should look like this:

```
timeout 60
default 0

title Windows XP - Work
unhide (hd0,0)
hide (hd0,1)
rootnoverify (hd0,0)
chainloader +1 

title Windows XP - Home
unhide (hd0,1)
hide (hd0,0)
rootnoverify (hd0,1)
chainloader +1
```

.

12\. Now unmount /dev/hda3, remove the linux CD and reboot. You should now get a grub boot menu where you can choose which copy of XP you want to load. The the hide and unhide commands for each OS entry in grub mean that the that each copy of windows won’t be able to see the other.

13\. (Optional) To add a nice background to the to the grub menu, boot back into your live Linux distro and use firefox to download a grub splash screen. I got one from [here](http://schragehome.de/splash/), they also have a guide to create your own. Again mount /dev/hda3 and copy the splashimage to /mnt/tmp/boot/grub. Edit the menu.list to include the following line:

```
splashimage=(hd0,3)/boot/grub/myfile.xpm.gz
```

.

That’s it! 2 Copies with windows compleatly hidden from each other wih a nice menu using the renound GNU GRUB bootloader!

<span style="color: #ff0000;"><span style="text-decoration: underline;">\*UPDATE\*</span></span>

<span style="color: #ff0000;">**DON’T USE HIBERNATE WITH DUAL BOOT.** </span>  
At first it seems neat to be able to choose which OS you want to resume but then the disk corruption starts!