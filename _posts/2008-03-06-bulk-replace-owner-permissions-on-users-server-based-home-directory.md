---
id: 72
title: 'Bulk replace owner / permissions on user&#8217;s server based home directory'
date: '2008-03-06T21:56:58+13:00'
author: RhysGoodwin
excerpt: 'How to easily replaces ownership and permissions on a folders using a simple script. Set ownership for users home folders.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=72'
permalink: /windows-admin/bulk-replace-owner-permissions-on-users-server-based-home-directory/
categories:
    - 'Windows Admin'
tags:
    - ntfs
    - 'ntfs ownership'
    - 'windows permissions'
---

This is useful if you want to start using quotas and all home drives are owned by local administrator. Or when you just want to tidy-up/reset access control on users home dirs.

### Bulk replace NTFS owner using folder name

– Get [subinacl.exe](http://www.microsoft.com/downloads/details.aspx?FamilyID=E8BA3E56-D8FE-4A91-93CF-ED6985E3927B&displaylang=en)  
-Run:  
for /d %%i in (\*) do subinacl /errorlog=subinacl.err /subdirectories %%i\*.\* /setowner=%%i

### Bulk replace NTFS permissions using folder name

 -Get [SetACL.exe](http://setacl.sourceforge.net/)  
-Run:  
for /D %%u in (\*.) DO SetACL.exe -on %%u -ot file -actn ace -ace “n:*domain*%%u;p:change” &gt;&gt;log.txt