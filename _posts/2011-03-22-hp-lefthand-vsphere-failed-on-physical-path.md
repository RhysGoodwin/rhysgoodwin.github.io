---
id: 1097
title: 'HP Lefthand / vSphere: &#8220;failed on physical path&#8221;'
date: '2011-03-22T00:14:42+13:00'
author: RhysGoodwin
excerpt: 'VMWare ESX / HP Lefthand - "Failed on physical path" error. "Have you tried turning it off and on?" '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1097'
permalink: /windows-admin/hp-lefthand-vsphere-failed-on-physical-path/
categories:
    - 'Windows Admin'
tags:
    - DSM
    - esx
    - iSCSI
    - Lefthand
    - MPIO
    - P4000
    - vmware
    - vSphere
---

We recently started having issues with our VMWare / HP Lefthand iSCSI SAN environment. The symptoms were as follows:

- VMs would sometime freeze-up for up to 10 seconds – no ping, nothing! Really nice on a busy SQL server running finance apps! Yeah! The problem affected VMs on both the Lefthand iSCSI and the fibre channel EVA
- Taking snapshots of VMs on the Lefthand storage would almost always fail and in most cases make a mess of disk chaining which would require manual clean up
- Browsing datastores is extremely slow
- General flakiness across the VI environment (Yes, that is a technical term)

I stated out by looking in the vmkernal logs of the ESX hosts and found errors like this occurring fairly regularly.

```
<span style="color: #ff0000;">Mar 10 18:04:02 myesxsvr01 vmkernel: 1:08:22:15.031 cpu1:4514)NMP: nmp_CompleteCommandForPath: Command 0x2a (0x4100040ebc00) to NMP device "naa.6000eb31749025160000000000016019" failed on physical path "vmhba33:C0:T14:L0" H:0x0 D:0x2 P:0x0 Valid sense data: 0x9 0x4 0x2.</span>
```

These errors were in relation to LUNs on the iSCSI SAN. A quick google of ***“failed on physical path H:0x0 D:0x2 P:0x0 Valid sense data: 0x9 0x4 0x2 Lefthand”*** quickly turned up this [VMWare KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1030129) which states that this is a LUN locking error caused by having VMFS LUNS presented to a Windows host which has the HP Lefthand DSM (Device Specific Module) for MPIO installed*.* This immediately rang a bell with me because we had recently installed a new backup server including full iSCSI MPIO support using the HP DSM.

Presenting the LUNs to the backup server allows VMs to be backed-up directly from the LUN as opposed to backing up via one of the ESX hosts. A good idea as long as you read the HP documentation all the way to the end and **don’t** install the DSM for MPIO!

[![](/content/uploads/2011/03/Lefthand-Physical-Path-Failure.jpg "Lefthand Physical Path Failures")](/content/uploads/2011/03/Lefthand-Physical-Path-Failure.jpg)

Great! I thought, I’ve found the problem. It appears the the LUN is being locked by the DSM and is causing the host to “timeout” affecting the entire storage subsystem (iSCSI and Fibre Channel). I went ahead and un-presented the iSCSI VMFS LUNs from the Windows host fully expecting the issues to clear up. Unfortunately this didn’t happen. My next step was to vMotion all the VMs off one host and reboot it. Still no luck, the errors returned to the vmkernal logs within a few minutes of the reboot.

At this point I logged a case with HP who provide our VMWare (and of course Lefthand) support. After they analysed the logs, they felt that the only way to resolve the issue was to do a full shutdown of the all the hosts and all the Lefthand storage! Classic support call – *“Have you tried turning it off and back on?”* But seriously, the guy at HP was very knowledgeable and helpful. We proved the approach as follows:

1. Create a new LUN on Lefthand and present it to all ESX hosts
2. Put a VM on the new LUN and prove that there are no issuers associated with the LUN by repeatedly taking snapshots and monitoring the vmkernal log
3. Present the LUN to the Windows backup host with the MPIO DSM. – Now the errors start occurring with this new LUN.
4. Un-present the LUN from ALL hosts (ESX and Windows)
5. Reboot one of the ESX hosts and re-present the new LUN to it. – The errors are no longer occurring with this LUN

It appears that access to a LUN from all hosts must be stopped to clear the locking so we did a fair amount of planning and undertook a full shutdown as follows:

1. Uninstall HP Lefthand DSM for MPIO from Windows hosts (We still want to try to present the VMFS LUNs back to the backup server at some stage)
2. Shutdown all VMs
3. Shutdown all the ESX hosts
4. Shutdown Lefthand (Shut down the management group, not the nodes individually)
5. Power up the Lefthand and make sure all the nodes are up and volumes are all online
6. Power up the ESX hosts and VMs

After doing this all LUN locking errors are gone from the logs. Everything seems very solid, snapshots are working and the flakiness is gone!

Any comments from anyone who has an understanding of the inner workings of iSCSI, lefthand, VMWare SCSI reservations/locking etc who can shed some light on what’s actually happening here would be much appreciated! Or just if you’ve had a similar experience I’d be keen to hear.

Thanks for reading