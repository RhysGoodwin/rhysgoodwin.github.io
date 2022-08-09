---
id: 914
title: 'Modular vbScript Framework'
date: '2010-12-14T18:36:23+13:00'
author: RhysGoodwin
excerpt: 'A simple modular vbscript framework which allows you to re-use code and maintain modules in isolation.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=914'
permalink: /windows-admin/modular-vbscript-framework/
categories:
    - 'Windows Admin'
tags:
    - 'code re-use'
    - functions
    - 'include file'
    - modular
    - scripting
    - vbs
    - vbscript
    - wsf
---

Here’s a modular vbscript framework I wrote for deploying software and performing other multi-step tasks across reboots.

I won’t list the whole script in the post because it will get messy with all the wrapping. Instead I’ll just describe the main features and you can download the zip file below. The screenshot will also give you a reasonable idea about how it works. It’s a bit rough and doesn’t have robust error handling – it’s really just to give you some ideas about putting together modular vbscripts.

[![](/content/uploads/2010/12/RGFramework.jpg "RGFramework")](/content/uploads/2010/12/RGFramework.jpg)

The main script is a **.wsf** (Windows script file). I use WSF because you can’t include other script files in a **.vbs** file. The framework gives us the following benefits:

- Code reuse – We can write core functions that are available across all modules
- Modular development – We can create and maintain modules in isolation
- Extensible – It’s easy to add new functions and modules
- Scripty goodness over unattended reboots/logons
- Log file generation

Here is the file/folder structure of the framework:

```
\RGFramework.wsf <em><span style="color: #0000ff;"><-Main script logic file</span></em>
\includes\header.vbs <span style="color: #0000ff;"><em><-Header file to declare variables etc</em></span>
\includes\Corefunc.vbs <span style="color: #0000ff;"><em><-</em>Functions which are available across all our modules </span>
\Modules\<em>(ModualName)</em>\<em>(ModualName.vbs) <span style="color: #0000ff;"><-Module files
</span></em><em><span style="color: #0000ff;"> </span></em>
```

### **Script logic**

Here’s an example of typical framework logic:

1. Start a continuous loop
2. Get the step which we’re up to from the registry. Registry key doesn’t exist yet (Step 0)
3. The Select statement executes the code for step 0 
    1. Set the script to run next time windows starts
    2. Set auto-logon so Windows logs on automatically when it boots up
    3. Run a module e.g. InstallOffice()
    4. Call “NextStepReboot()”. The current step is recorded in the registry and the system reboots
4. The system has rebooted and logged on automatically and the script starts again
5. Get the step which we’re up to from the registry (step 1)
6. The Select statement executes the code for step 1 
    1. Run another module .
    2. If we don’t need a reboot then we’ll just call NextStep() to record the step we’re up to
7. The code loops back around to the select statement and we now go to step 2 
    1. Run another module function
    2. Clear auto-logon
    3. clear script start-up
    4. exit

**Note\***

To start the script from the beginning again or from a specific step ‘the “HKLM\\SOFTWARE\\RGFramework\\Step” registry string must be deleted or manipulated. This string represents the **last step** that was run so if it is set to 3 then next the script runs it will execute step 4.

### **Modules**

A module is just a file which contains a single function. The function has the same name as the module file. Here is an example of a module which installs an application. *(\\modules\\installMyApp\\installMyApp*.vbs)

```
<span style="color: #0000ff;">Function </span><em>installMyApp</em>
objShell.run strInstallSource&"\MyApp\myapp.exe /quiet", 1, true
<span style="color: #0000ff;">End Function</span>
```

The module is added to the main script logic file with the following directive.

```
<script language="VBScript" src="<em>\modules\installMyApp\</em><em>installMyApp.vbs"</em>/>
```

The module is executed by simply calling the function during one of the steps.

```
installMyApp()
```

If you have any questions ask in the comments and let me know if you want more stuff like this 🙂

Here’s the zip file which contains the script structure and code.

[ RGFrameWork (1109 downloads) ](https://blog.rhysgoodwin.com/download/2457/ "Version 0.1")