---
id: 1562
title: 'OpenAM &#8211; Can&#8217;t Log on to Admin Console'
date: '2011-11-08T17:59:18+13:00'
author: RhysGoodwin
excerpt: 'FIXED:Unable to login to OpenAM/OpenSSO admin console after installation and config'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=1562'
permalink: /security/openam-cant-login-into-console/
categories:
    - Security
tags:
    - openam
    - opensso
    - tomcat
---

This one cost me some time!

**Symptoms**

After installing and configuring OpenAM you’re unable to log on to the admin console with the amAdmin account and password you set during the install. It doesn’t give an error message, just drops you back to the login page.

[![](/content/uploads/2011/11/OpenAMLogin.jpg "OpenAM Login")](/content/uploads/2011/11/OpenAMLogin.jpg)

**Cause**

When you go through the custom configuration wizard you get asked for the cookie domain. If your OpenAM server is **openam.mydomain.co.nz** then your cookie domain *should* be **.mydomain.co.nz** but by default the wizard just takes the trailing two domain components from the server name – i.e. **.co.nz**. Unless you specifically set the cookie domain correctly you’ll get the issue described above. As you can imagine this issue wouldn’t occur if your OpenAM server was called openam.mydomain.com.

[![](/content/uploads/2011/11/OpenAM.jpg "OpenAM  config Step 2")](/content/uploads/2011/11/OpenAM.jpg)

This means that if you have a domain name with more than 2 domain components then you’ll always need to run the custom config wizard.