---
id: 1254
title: 'ADFS 2.0 &#8211; Choose Your Attributes Wisely'
date: '2011-04-08T19:57:42+13:00'

excerpt: 'A potential security hole that can be unwittingly created in ADFS Web SSO implementation. '
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1254'
permalink: /security/adfs-2-0-choose-your-attributes-wisely/
embed:
    - ''
categories:
    - Security
tags:
    - 'active directory'
    - ADFS
    - attributes
    - federation
    - ldap
    - saml
    - Sas
    - sso
---

If you’ve read my last few posts you’ll be aware that I’m in the middle of implementing ADFS 2.0 for Web SSO. SalesForce for starters, with more to follow. I’m yet to put it into production but I was thinking today and just having a bit of a sanity check and something occurred to me. We send LDAP attributes as claims, the attributes are accepted by our service provider as law. They trust our federation service – that’s what federation is all about. Trust. There are number of mechanisms that make it very difficult for someone to spoof an assertion. On the whole, the SAML protocol can be considered very secure. What it can’t do is guarantee the validity of the source LDAP attribute.

[![](/content/uploads/2011/04/ClaimsRuleEditor.jpg "Claims Rule Editor")](/content/uploads/2011/04/ClaimsRuleEditor.jpg)

Consider the scenario above. We’re going to send the User’s telephone number as a claim. Maybe unlikely but it could happen, maybe you’ve got a SaS provider and you’ve already got 500 users in the system and telephone number is the only field you know is accurate between you and them. Unlikely? I know. But that’s not the point.

The issue is this – in Active Directory the attribute telephoneNumber, along with a few other attributes is by default, self writeable.

Once Dave figure’s out that the telephone number is significant he’ll update his phone number in AD to Bob’s phone number, launch the SaS app and will be logged in as Bob.

While there are only a few self writeable attributes in AD and they’re not ones you’d likely use for federation, it’s important to keep the whole picture in mind and the problem could go beyond self writeable attributes. A couple of other situations I can think of off the top of my head:

- Active directory self service applications which allows users to update attributes which aren’t normally self writeable
- Identity management systems which synchronise other systems to Active Directory. Not a problem in itself but you might be moving the point authorization for a specific application without realising it.

So choose your attributes wisely and make sure you know how, why, when and by whom or what they are written to before you decide to send them as federation claims.