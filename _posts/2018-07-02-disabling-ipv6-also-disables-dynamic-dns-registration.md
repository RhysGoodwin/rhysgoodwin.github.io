---
id: 2432
title: 'Disabling IPv6 Also disables Dynamic DNS Registration'
date: '2018-07-02T16:30:42+13:00'
author: RhysGoodwin
excerpt: 'In Windows 7 and Windows 10 disabling IPv6 also disables dynamic DNS registration. I guess it''s a bug?'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=2432'
permalink: /uncategorized/disabling-ipv6-also-disables-dynamic-dns-registration/
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
    - Uncategorized
tags:
    - DNS
    - 'dns registration'
    - 'dynamic update'
---

Dynamic DNS updates not happening at boot or when doing an ipconfig release/renew. But manual ipconfig /registerdns works fine. Tracked this down to IPv6 being disabled by GPO. I don’t know the reason for this. but Microsfot do state:

***Important** Internet Protocol version 6 (IPv6) is a mandatory part of Windows Vista and Windows Server 2008 and newer versions. We do not recommend that you disable IPv6 or its components. If you do, some Windows components may not function. We recommend that you use “Prefer IPv4 over IPv6” in prefix policies instead of disabling IPV6.*

```
Policy: Computer\Polices\Administrative Templates\Network\IPv6 Configuration
Registry: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters\DisabledComponents(DWORD)=0xff
```

[![](/content/uploads/2018/07/IPv6GPO.png)](/content/uploads/2018/07/IPv6GPO.png)

Worth noting:

- Reboot is required for the change to take effect (at least for the dynamic registration behavior to change)
- De-scoping this policy (or setting it to not configured) doesn’t revert IPv6 to enabled. Instead you need to configure the setting “Enable all IPv6 components” and then de-scope the policy after the change as taken effect
- Ticking *“Use this connection’s DNS suffix in DNS registration”* causes dynamic registration to **work as normal**, even when IPv6 is disabled. In my testing the primary and the connection specific DNS suffix was the same
- Consider using *‘Prefer IPv4 over IPv6*‘ instead of disabling IPv6 as this does not impact Dynamic DNS updates
- I’m only talking about DHCP clients here not static clients. The behavior may be different for static clients

[![](/content/uploads/2018/07/NicDNSSettings.png)](/content/uploads/2018/07/NicDNSSettings.png)