---
id: 887
title: 'cPanel Says the Subdomain Cannot be Removed. but it can!'
date: '2010-12-10T15:45:06+13:00'

excerpt: 'An easy way to get rid of subdomains when cPanel says you can''t!  Do you get this error: "Sorry, the subdomain cannot be removed because it is linked to the addon domain"?'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=887'
permalink: /hosting/cpanel-says-the-subdomain-cannot-be-removed-but-it-can/
categories:
    - Hosting
tags:
    - bluehost
    - cpanel
    - hosting
    - subdomain
---

While assigning a new add-on domain to Bluehost I came across this problem. When you sign up with Bluehost (and I suspect any other cPanel hoster) your account is associated with a primary domain (the first one you signed up with). If you then add additional domains to your account cPanel decides to add subdomains to your primary domain. Like this:

```
mysite.com (Primary cPanel domain)
someothersite.com (Domain you added to cPanel)
someothersite.mysite.com (subdomain added by cPanel - <em>yeah thanks for that?!</em>)
```

This is really bad for SEO because you end up with 2 different domains pointing to the same content. If you try to remove it using the cPanel Subdomains tool it throws this error:

[](/content/uploads/2010/12/Bluehost-cPanel.jpg)[![](/content/uploads/2010/12/Bluehost-cPanel.jpg "Bluehost cPanel")](/content/uploads/2010/12/Bluehost-cPanel.jpg)

**There was a problem removing the subdomain.**  
<span style="color: #ff0000;">Sorry, the subdomain “someothersite” cannot be removed because it is  
linked to the addon domain “someothersite.com”. You must first remove the addon domain.</span>

What? why? Ok whatever. To get rid of this unwanted subdomain you need to use the “Advanced DNS Zone Editor”.

[![](/content/uploads/2010/12/BluehostAdvancedDNSZoneEditor.jpg "Bluehost Advanced DNS Zone Editor")](/content/uploads/2010/12/BluehostAdvancedDNSZoneEditor.jpg)

After you delete the subdomain record you’ll still see the subdomain listed in in the subdomains tool. You won’t be able to get rid of it from there, however it will be gone from the DNS system so it won’t be accessible or crawlable by Google.

I do want to make it clear that this isn’t a problem specific to Bluehost – it’s a cPanel issue. I actually really like Bluehost. If you’re looking for hosting I really recommended them. This blog is hosted with them and it just works! Unlike other hosters which I’ve found to be painfully slow.

[So go over and check them out!](http://www.bluehost.com/track/rhysgood/post1)

Take that cPanel!