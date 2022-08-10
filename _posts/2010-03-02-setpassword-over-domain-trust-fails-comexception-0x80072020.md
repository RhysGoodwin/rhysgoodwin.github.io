---
id: 494
title: 'SetPassword over domain trust fails &#8211; COMException 0x8007202'
date: '2010-03-02T21:30:36+13:00'

excerpt: 'A different .NET method for setting passwords in an AD environment over LDAP.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=494'
permalink: /programming/setpassword-over-domain-trust-fails-comexception-0x80072020/
categories:
    - Programming
tags:
    - '0x80072020'
    - 'active directory'
    - COMException
    - delegation
    - direcotryserivces
    - impersonation
    - 'invoke setpassword'
    - kerberos
    - ldap
    - ldaps
    - setpassword
    - trust
---

After breaking my head over this for about 3 days and finally coming up with a solution I thought Id share.[![](/content/uploads/2010/03/COMException-0x80072020.png "COMException 0x80072020")](/content/uploads/2010/03/COMException-0x80072020.png)

Here’s the situation:

- 2 Forests with a one way external trust between them. 
    - Domain B in Forest B trusts Domain A in forest A.
- An ASP.NET Application running in Domain A 
    - Configured to impersonate.
    - Application Pool running under a service account from domain A which is trusted for delegation
    - Application URL (DNS name) is registered as an SPN against the above service account used to the application pool.
- A user (Bob) from Domain A has permission to modify properties and reset passwords of users in Domain B
- When using the Active Directory users and computers MMC Bob is able to reset passwords and modify properties of users in the trusting domain(Domain B) I.e. Permissions are ok!
- When Bob uses the ASP.NET application (System.DirectoryServices.DirectoryEntry) Bob is able to modify properties of users in the trusting domain. I.e. Bob’s creditneials are sucessfuly delegated to the domain controller in the trusitng domain. I.e. Delegation is working!
- When the ASP.NET application specifies Bob’s username/password in the DirectoryEntry object e.g. <span style="color: #3366ff;">DirectoryEnty de = new DirectoryEntry(“LDAP://domainB.com</span><span style="color: #3366ff;">/CN=Steve,ou=test,dc=domainb,dc=com”,”bob”,”password1″)</span> <span style="color: #000000;"> then invokes the setpassword method, the password is successfully set. I.e. SSL is working correctly. Necessary firewall ports are open etc</span>
- <span style="color: #000000;">When Bob uses the ASP.NET application and invokes the setpassword method now using delegation a COMException 0x80072020 is thrown.</span>

<span style="color: #000000;">I’ve spent some hours trying to work this out, network dumps with WireShark (no good for LDAPS), Kerberos logging on DCs and webserver, etc etc. I have also opened up the firewall to allow DirectoryEntry.Invoke(“setpassword”) to try his other methods. I just can’t get this to work in a domain trust environment with delegation. I have however found that using System.DirectoryServices.Protocols to reset the password in a trusting domain with a delegated credential does work! For this I’m very relieved; I was almost at the point where I was going to go back to our dev guys and tell them to switch back to using a superuser account instead of the delegation method which I had been pushing on them for weeks!</span>

<span style="color: #000000;">Frustrating though that I still don’t know why this method works where the standard System.DirectoryServices method doesn’t!</span>

<span style="color: #000000;">This example was taken from chapter 10.16 of</span> <span style="color: #3366ff;">*[The .NET Developer’s Guide to Directory Services Programming](http://www.amazon.com/gp/product/0321350170?ie=UTF8&tag=blogrhysgoodw-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=0321350170)![](https://www.assoc-amazon.com/e/ir?t=blogrhysgoodw-20&l=as2&o=1&a=0321350170)* <span style="color: #000000;">by Joe Kaplan and Ryan Dunn.</span></span><span style="color: #000000;"> I strongly recommend you get this book. It’s not the newest book out but it’s still packed with really relevant stuff and lots of “gold nuggets” from 2 guys who really know their stuff.  
</span>

```
using System;
using System.DirectoryServices.Protocols;
using System.Net;
using System.Text;

public class PasswordModifier
{
    public static void Main()
    {
        NetworkCredential credential = new NetworkCredential(
            "someuser",
            "Password1",
            "domain"
            );
        DirectoryConnection connection;

        try
        {

            //change these options to use Kerberos encryption
            connection = GetConnection(
                "domain.com:636",
                credential,
                true
                );

            ChangePassword(
                connection,
                "CN=someuser,CN=users,DC=domain,DC=com",
                "Password1",
                "Password2"
                );

            Console.WriteLine("Password modified!");
            IDisposable disposable = connection as IDisposable;

                if (disposable != null)
                    disposable.Dispose();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
            }
        }

        private static DirectoryConnection GetConnection(
            string server,
            NetworkCredential credential,
            bool useSsl
            )
        {
            LdapConnection connection =
               new LdapConnection(server);

            if (useSsl)
            {
                connection.SessionOptions.SecureSocketLayer = true;
            }
            else
            {
               connection.SessionOptions.Sealing = true;
            }

            connection.Bind(credential);
            return connection;
        }

        private static void ChangePassword(
            DirectoryConnection connection,
            string userDN,
            string oldPassword,
            string newPassword
            )
        {
            DirectoryAttributeModification deleteMod =
                new DirectoryAttributeModification();
            deleteMod.Name = "unicodePwd";
            deleteMod.Add(GetPasswordData(oldPassword));
            deleteMod.Operation= DirectoryAttributeOperation.Delete;

            DirectoryAttributeModification addMod =
                new DirectoryAttributeModification();
            addMod.Name = "unicodePwd";
            addMod.Add(GetPasswordData(newPassword));
            addMod.Operation = DirectoryAttributeOperation.Add;

        ModifyRequest request = new ModifyRequest(
            userDN,
            deleteMod,
            addMod
            );

        DirectoryResponse response =
            connection.SendRequest(request);
    }

    private static void SetPassword(
        DirectoryConnection connection,
        string userDN,
        string password<a name="iddle1098"></a><a name="d0e42129"></a>
        )
    {

        DirectoryAttributeModification pwdMod =
            new DirectoryAttributeModification();
        pwdMod.Name = "unicodePwd";
        pwdMod.Add(GetPasswordData(password));
        pwdMod.Operation = DirectoryAttributeOperation.Replace;

        ModifyRequest request = new ModifyRequest(
            userDN,
            pwdMod
            );

        DirectoryResponse response =
            connection.SendRequest(request);

    }

    private static byte[] GetPasswordData(string password)
    {
        string formattedPassword;
        formattedPassword = String.Format("\"{0}\"", password);
        return (Encoding.Unicode.GetBytes(formattedPassword));
    }
}
```

In my case this is somehow related to the domain trust but I guess any time 0x8007202 appears during setpassword and you just can’t get it to go away I’d suggest giving this method a go!