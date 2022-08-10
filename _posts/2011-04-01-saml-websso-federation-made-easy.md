---
id: 1126
title: 'SAML WebSSO &#038; Federation Made Easy'
date: '2011-04-01T11:58:06+13:00'

excerpt: 'SAML (Security Assertion Mark-up Language) and Federation Made Easy!'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1126'
permalink: /security/saml-websso-federation-made-easy/
categories:
    - Security
tags:
    - Authentication
    - cloud
    - federation
    - saml
    - security
    - sso
    - websso
---

This week I found myself on a SalesForce/SAML/Federation journey which turned out to be very enlightening. Until a few days ago I really had no idea how SAML or Federation worked and it took me a few hours to get my head around it, so I’m going to try explain SAML in a way that’s easy to understand.

**SAML 2.0 (Security Assertion Markup Language).**

2 Companies:  
Company A (Service Provider – **SP**) has a web application  
Company B (Identity Provider – **IdP**) has a database of people who need to access Company A’s application

We have a few options here:

1. Company A could create a new database of people with usernames and passwords within the web application
2. We could synchronise the database of people including their usernames and passwords from Company B to Company A
3. We could make a link from the web application to Company B’s database of people and do lookups in real-time
4. We could tell the web application at Company A to trust users who come from Company B

Options 1 through 3 are pretty crappy. Option 4 is called federation and it’s cool.

Here’s what happens (part analogy, part reality):

Both companies have a pair of keys. A public key and a private key. Once something is locked with the private key only the corresponding public key can open it. Company A has a copy of Company B’s public key and vice versa.

A user in Company B tries to access the web application at Company A. The web application looks for a cookie in the user’s browser to see if he is already authenticated, he is not so the web application (SP) redirects the browser to Company B’s IdP, telling him – “Go and get a ticket!”

The browser goes to Company B’s IdP who authenticates the user against Company B’s database of users. The IdP at Company B locks the user’s employee ID with his **private** key, gives it to the browser and tells him – “Here’s your ticket now go back to the SP you came from!”

The browser goes back to the web application (SP) at Company A and presents his ticket. The SP uses Company B’s **public** key to unlock the ticket. The web application says to himself – *“It works! This user MUST have come from Company B because otherwise this public key could NOT have unlocked this ticket. And look, the ticket contains an employee ID and I have a rule that says that this* employee ID *is allowed access!*” And so the web application gives the browser a cookie which allows him access.

In SAML the ticket is called an Assertion. In this case we sent the Employee ID but any other user-unique attribute could be used, it just needs to be agreed between the 2 parties.

In reality the web application might not support SAML directly but instead maybe protected by a federation product which takes care of the SAML SP stuff. The IdP stuff will also likely be handled by a federation product which is backed by some kind of LDAP directory or maybe SQL. The browser cookie stuff mentioned above is outside the scope of SAML but I included it for completeness – It’s typical of how these things work.

The neat thing about federation is that you don’t need any links between Company A and Company B. Once the trust is established everything else takes place “browser to SP” and “browser to IdP” through a series or re-directs and http POSTs.

Ok so that’s SAML. Actually there are a lot more parts to it than that but that’s the way it’s most commonly used today i.e. for WebSSO. Now that you have the concept, you can dig into the technical details.

I found Google’s explanation useful:  
[http://code.google.com/googleapps/domain/sso/saml\_reference\_implementation.html](http://code.google.com/googleapps/domain/sso/saml_reference_implementation.html)

and Wikipedia :

[http://en.wikipedia.org/wiki/SAML\_2.0](http://en.wikipedia.org/wiki/SAML_2.0)

The best way to learn this stuff is to give it a go. Check out [Shibboleth ](http://shibboleth.internet2.edu/)which is an open source SAML SP and IdP implementation. I’ve got the Shibboleth SP side talking to ADFS 2.0 as the IdP but I haven’t played with the Shibboleth IdP yet.

Next time I’ll show you how to put SAML to use with Active Directory Federation Services 2.0 and cloud provider SalesForce. In the mean time feel free to ask questions or make corrections.