---
id: 874
title: 'Cheatsheet: Add HP Lefthand Storage Nodes'
date: '2010-12-08T17:37:19+13:00'

excerpt: 'Cheatsheet: Add new HP Lefthand (StorageWorks P4000) nodes to your iSCSI SAN.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=874'
permalink: /windows-admin/cheatsheet-add-hp-lefthand-storage-nodes/
categories:
    - 'Windows Admin'
tags:
    - CMC
    - HP
    - iSCSI
    - Lefthand
    - P4000
    - P4500
    - P4500G2
    - SAN
    - 'SAN IQ'
    - storage
    - virtualization
---

Here are the quick high level steps for adding new storage nodes (P4000, P5400 etc) to your HP Lefthand iSCSI SAN (AKA StorageWorks P4000).[![](/content/uploads/2010/12/P4500G2.jpg "HP Lefthand P4500 G2 ")](/content/uploads/2010/12/P4500G2.jpg)

1. Install and cable the units
2. Check the HP site for firmware updates and apply ONLY the recommended updates
3. Configure iLO so you don’t have to spend so much time in the cold room (optional)
4. At the console on each unit give the first NIC an IP address. You can leave the other one disabled for now.
5. Download and install the latest [SAN/iQ Centralized Management Console](http://www.hp.com/go/P4000downloads) on your workstation or management server
6. In the CMC add the new nodes by going “Find Systems” and entering the IP addresses you assigned to the nodes
7. Under “Available devices” go to the TCP/IP settings of each node and create a bond so the two NICs become one and choose a load balancing type.
8. Go to <http://webware.hp.com/> and generate your license keys. Each unit comes with an entitlement certificate. You’ll need to provide the Feature Key (MAC Address) which can be found in the CMC under “Feature Registration” for each node. When you get the key replace the one that’s in there by default.
9. Right click the units and add them to an existing management group (or create a new one)
10. Now that You have the units in the management group add them to an existing cluster (or create a new one)

Nodes must be of equal or greater capacity to existing nodes in a cluster. If they are of greater capacity then only the capacity of the smallest node in the cluster will be usable – Maybe time to create a new cluster?

I’ll let you fill in the detail but that’s basically it.