---
id: 144
title: 'IE GPO Zone Templates and the &#8220;Open File &#8211; Security Warning&#8221;'
date: '2009-02-09T23:20:24+13:00'
author: RhysGoodwin
excerpt: "Do you get Open File - Security Warning? Do you get strange results when trying to manage Internet Explorer security zones with group policy? Read This!\r\n\r\n"
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=144'
permalink: /windows-admin/ie-gpo-zone-templates-and-the-open-file-security-warning/
categories:
    - 'Windows Admin'
tags:
    - GPO
    - 'Internet explorer'
    - 'Open file security warning'
    - 'Security Zones'
    - 'trusted zone'
    - 'zone templates'
---

Since you’re reading this you probably already know that Internet Explorer has a number of security zones. URLs are treated differently depending on the zone they fall into. These security zones apply not just to URLs in Internet Explorer but to windows in general e.g. accessing files from a network location. My specific problem was a GPO start-up script that ran backinfo to display the server info on the desktop when an admin user logs on. Backinfo.exe an unsigned application stored on the netlogon share would throw the **Open File – Security Warning** every time it was launched. More about that soon.

![Open File Security Warning](/content/uploads/2009/04/openfilewarning.jpg "openfilewarning")

In the enterprise it’s desirable to configure all these zone and security settings using group policy but there are a few gotchas that can make the configuration and testing process a bit confusing.

[![gpo](/content/uploads/2009/04/gpo.jpg "gpo")](/content/uploads/2009/04/gpo.jpg)

A standard zone template can be applied to a user’s settings. After you apply this template you can do a **gpupdate /force/target:user**; you **won’t** get a warning about logging on/off. Now in Internet Explorer you’ll notice a couple of things. (1)The security level and visual slider for the zone on the security page will not have changed and will not reflect the template you’ve selected in the GPO. (2)If you click on “Custom level” you’ll see that the individual settings that the selected template represents are in fact set and are now unchangeable, i.e. the policy has applied.  
[![securitypage](/content/uploads/2009/04/securitypage.jpg?w=300 "securitypage")](/content/uploads/2009/04/securitypage.jpg?w=300)

Ok so at this point we could be forgiven for assuming that the policy has been fully applied to the system; we can see the changes in IE and we know that gpupadte **didn’t** ask us to log off/on.

Now on to the “Open File – Security Warning “, this is affected by the setting pictured above, “Launching applications and unsafe files”. Since this *is* a trusted zone we trust all the locations in this zone so we are happy to launch unsigned applications without a security warning. For some strange reason this setting is one of the only ones that can’t be set individually with group policy, the only way to set it (via GPO) is to apply a template as described above. Both “Low” and “Medium Low” will allow applications to launch from a network location without a security warning.

The thing that’s really confusing is that even though doing the gpupdate updates the policy in IE it is not fully applied to the reset of the system until you log off/on.

In Conclusion

- Security templates are not visually reflected in the security page of Internet Explorer even though they *are* applied.
- Security zone settings are applied to Internet Explorer by doing a gpupdate but a log off/on is required to apply these settings to the rest of the OS
- The **“Launching applications and unsafe files”** setting determines whether the **“Open File – Security Warning”** dialog is displayed when launching applications from a given location
- The **“Launching applications and unsafe files”** cannot be set with a an indvidual GPO setting. (You could create a custom adm file though)
- When setting zone security via GPO I recommend making the Internet Explorer security page invisible to users to avoid confusion as they can still quite happily adjust the security level slider, it just won’t have any effect!