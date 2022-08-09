---
id: 997
title: 'Citrix Web Interface with ISA Single Sign On'
date: '2011-02-17T20:16:52+13:00'
author: RhysGoodwin
excerpt: 'How to configure single-sign-on for the Citrix Web Interface/Secure Gateway with Microsoft ISA server.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=997'
permalink: /windows-admin/citrix-web-interface-with-isa-single-sign-on/
embed:
    - ''
wp-syntax-cache-content:
    - "a:2:{i:1;s:2260:\"\n<div class=\"wp_syntax\" style=\"position:relative;\"><table><tr><td class=\"code\"><pre class=\"asp\" style=\"font-family:monospace;\"><span style=\"color: #000000; font-weight: bold;\">&lt;%</span><span style=\"color: #006600; font-weight: bold;\">@</span> Page Language<span style=\"color: #006600; font-weight: bold;\">=</span><span style=\"color: #cc0000;\">&quot;C#&quot;</span> AutoEventWireup<span style=\"color: #006600; font-weight: bold;\">=</span><span style=\"color: #cc0000;\">&quot;true&quot;</span> CodeFile<span style=\"color: #006600; font-weight: bold;\">=</span><span style=\"color: #cc0000;\">&quot;AuthPass.aspx.cs&quot;</span> Inherits<span style=\"color: #006600; font-weight: bold;\">=</span><span style=\"color: #cc0000;\">&quot;AuthPass&quot;</span> <span style=\"color: #000000; font-weight: bold;\">%&gt;</span>\n&nbsp;\n&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD XHTML 1.0 Transitional//EN&quot; &quot;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&quot;&gt;\n&nbsp;\n&lt;html xmlns=&quot;http://www.w3.org/1999/xhtml&quot;&gt;\n\t&lt;head&gt;\n\t\t&lt;title&gt;\n\t\t&lt;/title&gt;\n\t&lt;/head&gt;\n\t&lt;body onload=&quot;submitlogin()&quot;&gt;\n\t\t&lt;div id=&quot;FormContainer&quot; runat=&quot;server&quot;&gt;\n&nbsp;\n\t\t&lt;/div&gt;\n\t&lt;/body&gt;\n&lt;/html&gt;\n&nbsp;\n&lt;script type=&quot;text/javascript&quot; language=&quot;javascript&quot;&gt;\n\t function submitlogin()\n\t {\n\t\tdocument.CitrixForm.submit();\n\t }\n&lt;/script&gt;</pre></td></tr></table><p class=\"theCode\" style=\"display:none;\">&lt;%@ Page Language=&quot;C#&quot; AutoEventWireup=&quot;true&quot; CodeFile=&quot;AuthPass.aspx.cs&quot; Inherits=&quot;AuthPass&quot; %&gt;\r\n\r\n&lt;!DOCTYPE html PUBLIC &quot;-//W3C//DTD XHTML 1.0 Transitional//EN&quot; &quot;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&quot;&gt;\r\n\r\n&lt;html xmlns=&quot;http://www.w3.org/1999/xhtml&quot;&gt;\r\n\t&lt;head&gt;\r\n\t\t&lt;title&gt;\r\n\t\t&lt;/title&gt;\r\n\t&lt;/head&gt;\r\n\t&lt;body onload=&quot;submitlogin()&quot;&gt;\r\n\t\t&lt;div id=&quot;FormContainer&quot; runat=&quot;server&quot;&gt;\r\n\r\n\t\t&lt;/div&gt;\r\n\t&lt;/body&gt;\r\n&lt;/html&gt;\r\n\r\n&lt;script type=&quot;text/javascript&quot; language=&quot;javascript&quot;&gt;\r\n\t function submitlogin()\r\n\t {\r\n\t\tdocument.CitrixForm.submit();\r\n\t }\r\n&lt;/script&gt;</p></div>\n\";i:2;s:8877:\"\n<div class=\"wp_syntax\" style=\"position:relative;\"><table><tr><td class=\"code\"><pre class=\"csharp\" style=\"font-family:monospace;\"><span style=\"color: #0600FF; font-weight: bold;\">using</span> <span style=\"color: #008080;\">System</span><span style=\"color: #008000;\">;</span>\n<span style=\"color: #0600FF; font-weight: bold;\">using</span> <span style=\"color: #008080;\">System.Net</span><span style=\"color: #008000;\">;</span>\n<span style=\"color: #0600FF; font-weight: bold;\">using</span> <span style=\"color: #008080;\">System.Text</span><span style=\"color: #008000;\">;</span>\n<span style=\"color: #0600FF; font-weight: bold;\">using</span> <span style=\"color: #008080;\">System.Web</span><span style=\"color: #008000;\">;</span>\n&nbsp;\n<span style=\"color: #0600FF; font-weight: bold;\">public</span> <span style=\"color: #0600FF; font-weight: bold;\">partial</span> <span style=\"color: #6666cc; font-weight: bold;\">class</span> AuthPass<span style=\"color: #008000;\">:</span> <span style=\"color: #000000;\">System.<span style=\"color: #0000FF;\">Web</span><span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">UI</span></span><span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Page</span>\n<span style=\"color: #008000;\">&#123;</span>\n    <span style=\"color: #0600FF; font-weight: bold;\">protected</span> <span style=\"color: #6666cc; font-weight: bold;\">void</span> Page_Load<span style=\"color: #008000;\">&#40;</span><span style=\"color: #6666cc; font-weight: bold;\">object</span> sender, EventArgs e<span style=\"color: #008000;\">&#41;</span>\n    <span style=\"color: #008000;\">&#123;</span>\n\t\tFormContainer<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">InnerHtml</span> <span style=\"color: #008000;\">=</span> doLogin<span style=\"color: #008000;\">&#40;</span>Request<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">ServerVariables</span><span style=\"color: #008000;\">&#91;</span><span style=\"color: #666666;\">&quot;AUTH_USER&quot;</span><span style=\"color: #008000;\">&#93;</span>,Request<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">ServerVariables</span><span style=\"color: #008000;\">&#91;</span><span style=\"color: #666666;\">&quot;AUTH_PASSWORD&quot;</span><span style=\"color: #008000;\">&#93;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t<span style=\"color: #008000;\">&#125;</span>\n&nbsp;\n\t<span style=\"color: #0600FF; font-weight: bold;\">private</span> <span style=\"color: #6666cc; font-weight: bold;\">String</span> doLogin<span style=\"color: #008000;\">&#40;</span><span style=\"color: #6666cc; font-weight: bold;\">String</span> strUser, <span style=\"color: #6666cc; font-weight: bold;\">String</span> strPassword<span style=\"color: #008000;\">&#41;</span>\n\t<span style=\"color: #008000;\">&#123;</span>\n\t\tStringBuilder strForm <span style=\"color: #008000;\">=</span> <span style=\"color: #008000;\">new</span> StringBuilder<span style=\"color: #008000;\">&#40;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\tstrForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Append</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #666666;\">&quot;&amp;lt;form name=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>CitrixForm<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> action=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>https://citrix.corp.com/Citrix/XenApp/auth/login.aspx<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> method=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>post<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>&amp;gt;&quot;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\tstrForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Append</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #666666;\">&quot;&amp;lt;input type=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>hidden<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> name=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>domain<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> value=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>MyDomain<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>&amp;gt;&quot;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\tstrForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Append</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #666666;\">&quot;&amp;lt;input type=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>hidden<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> name=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>user<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> value=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>{0}<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>&amp;gt;&quot;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\tstrForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Append</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #666666;\">&quot;&amp;lt;input type=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>hidden<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> name=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>password<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> value=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>{1}<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>&amp;gt;&quot;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\tstrForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Append</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #666666;\">&quot;&amp;lt;input type=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>hidden<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> name=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>LoginType<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span> value=<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>Explicit<span style=\"color: #008080; font-weight: bold;\">\\&quot;</span>&amp;gt;&quot;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\tstrForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Append</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #666666;\">&quot;&amp;lt;/form&amp;gt;&quot;</span><span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t\t<span style=\"color: #0600FF; font-weight: bold;\">return</span> <span style=\"color: #6666cc; font-weight: bold;\">String</span><span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">Format</span><span style=\"color: #008000;\">&#40;</span>strForm<span style=\"color: #008000;\">.</span><span style=\"color: #0000FF;\">ToString</span><span style=\"color: #008000;\">&#40;</span><span style=\"color: #008000;\">&#41;</span>, strUser, strPassword<span style=\"color: #008000;\">&#41;</span><span style=\"color: #008000;\">;</span>\n\t<span style=\"color: #008000;\">&#125;</span>\t\n&nbsp;\n<span style=\"color: #008000;\">&#125;</span></pre></td></tr></table><p class=\"theCode\" style=\"display:none;\">using System;\r\nusing System.Net;\r\nusing System.Text;\r\nusing System.Web;\r\n\r\npublic partial class AuthPass: System.Web.UI.Page\r\n{\r\n    protected void Page_Load(object sender, EventArgs e)\r\n    {\r\n\t\tFormContainer.InnerHtml = doLogin(Request.ServerVariables[&quot;AUTH_USER&quot;],Request.ServerVariables[&quot;AUTH_PASSWORD&quot;]);\r\n\t}\r\n\r\n\tprivate String doLogin(String strUser, String strPassword)\r\n\t{\r\n\t\tStringBuilder strForm = new StringBuilder();\r\n\t\tstrForm.Append(&quot;&amp;lt;form name=\\&quot;CitrixForm\\&quot; action=\\&quot;https://citrix.corp.com/Citrix/XenApp/auth/login.aspx\\&quot; method=\\&quot;post\\&quot;&amp;gt;&quot;);\r\n\t\tstrForm.Append(&quot;&amp;lt;input type=\\&quot;hidden\\&quot; name=\\&quot;domain\\&quot; value=\\&quot;MyDomain\\&quot;&amp;gt;&quot;);\r\n\t\tstrForm.Append(&quot;&amp;lt;input type=\\&quot;hidden\\&quot; name=\\&quot;user\\&quot; value=\\&quot;{0}\\&quot;&amp;gt;&quot;);\r\n\t\tstrForm.Append(&quot;&amp;lt;input type=\\&quot;hidden\\&quot; name=\\&quot;password\\&quot; value=\\&quot;{1}\\&quot;&amp;gt;&quot;);\r\n\t\tstrForm.Append(&quot;&amp;lt;input type=\\&quot;hidden\\&quot; name=\\&quot;LoginType\\&quot; value=\\&quot;Explicit\\&quot;&amp;gt;&quot;);\r\n\t\tstrForm.Append(&quot;&amp;lt;/form&amp;gt;&quot;);\r\n\t\treturn String.Format(strForm.ToString(), strUser, strPassword);\r\n\t}\t\r\n\r\n}</p></div>\n\";}"
categories:
    - 'Windows Admin'
tags:
    - basic
    - Citrix
    - CSG
    - explicit
    - 'HTTP Authentication'
    - ISA
    - 'pass through'
    - 'secure gateway'
    - sso
    - Tmg
    - 'web interface'
    - wi
---

It’s been a long time since my last post! I’ve been so busy working on the house (but nothing really blog-worthy). Anyway today a colleague and I went through and set up the Citrix Web Interface (5.x) with single-sign-on using Microsoft ISA 2006.

The Web Interface and Secure Gateway run on the same server but are configured completely independently of each other, they could just as well be on separate servers if the load warranted it. They both listen on port 443 on separate IP addresses with separate certificates.

[![](/content/uploads/2011/02/CSG-SSO.png "Citrix Secure Gateway With ISA SSO")](/content/uploads/2011/02/CSG-SSO.png)

On the face of it, it seems quite straight forward – configure the Web Interface for pass-through authentication, create an ISA web publishing rule using our common SSO web listener with forms based authentication and configure an authentication delegation method. This works just fine as far as getting the user logged in with their list of applications.

Next step – configure the CSG to listen on a separate IP address with a separate certificate and configure a NAT rule so the ICA client can connect directly to the CSG. Again fairly straight forward.

**Here’s the catch.** Using pass-through on the web interface doesn’t work with the CSG. Pass-through mode expects the client to be domain-joined, inside the corporate network and able to authenticate directly with the XenApp Server (as opposed to being pre-authenticated by the XML/STA services). The result with the above configuration is that when the user launches an application they are presented with a Windows login dialog which defeats the purpose of single-sign-on.

**The solution – ASP.Net “jump” page on the web interface.**

Configure the Web Interface in “Explicit ” mode rather than pass-through. This is the standard method where the user is presented with the Citrix Web Interface login form.

Configure the ISA web publishing rule to delegate “basic” credentials. i.e. clear text user-name/password (secured with SSL of course!).

Create an ASP.NET jump page which extracts the user-name and password from the HTTP request, and creates a form with hidden fields then uses java script to POST the form to the Web Interface login page.

This all happens instantly without the user noticing. Don’t configure the Web Interface as the default IIS page, instead place the jump page in the root of the IIS web site and set the document priority to to serve it up first. Here’s the code: <span style="color: #3366ff;">*(Download link at the end of the post)* </span>

**AuthPass.aspx**

```
<pre escaped="true" lang="asp"><%@ Page Language="C#" AutoEventWireup="true" CodeFile="AuthPass.aspx.cs" Inherits="AuthPass" %>



<html xmlns="http://www.w3.org/1999/xhtml">
	<head>
		<title>
		</title>
	</head>
	<body onload="submitlogin()">
		<div id="FormContainer" runat="server">

		</div>
	</body>
</html>

<script type="text/javascript" language="javascript">
	 function submitlogin()
	 {
		document.CitrixForm.submit();
	 }
</script>
```

**AuthPass.aspx.cs** – <span style="color: #ff0000;">Note:</span> The domain field needs to be set to your own domain or removed completely depending on how your users login. The form action needs to point to your Web Interface login.aspx page.

```
<pre lang="csharp">using System;
using System.Net;
using System.Text;
using System.Web;

public partial class AuthPass: System.Web.UI.Page
{
    protected void Page_Load(object sender, EventArgs e)
    {
		FormContainer.InnerHtml = doLogin(Request.ServerVariables["AUTH_USER"],Request.ServerVariables["AUTH_PASSWORD"]);
	}

	private String doLogin(String strUser, String strPassword)
	{
		StringBuilder strForm = new StringBuilder();
		strForm.Append("<form name=\"CitrixForm\" action=\"https://citrix.corp.com/Citrix/XenApp/auth/login.aspx\" method=\"post\">");
		strForm.Append("<input type=\"hidden\" name=\"domain\" value=\"MyDomain\">");
		strForm.Append("<input type=\"hidden\" name=\"user\" value=\"{0}\">");
		strForm.Append("<input type=\"hidden\" name=\"password\" value=\"{1}\">");
		strForm.Append("<input type=\"hidden\" name=\"LoginType\" value=\"Explicit\">");
		strForm.Append("</form>");
		return String.Format(strForm.ToString(), strUser, strPassword);
	}	

}
```

Here are the key points.

**ISA 2006**

- Web publishing rule with SSO WebListener using forms based Authentication
- “Basic” authentication delegation (SSL end to end!)
- Published logoff URL is set to **/Citrix/XenApp/site/logout.aspx**
- Simple NAT rule for CSG

**Web Interface**

- A new XenApp site is created in the Web Interface Management tool with “*At Web Interface*” configured for the “*Where user authentication takes place*” setting
- Authentication Method set to Explicit
- AuthPass.aspx\[.cs\] files are placed in the root of the IIS website to handle auto-login
- XenApp web site is not configured as the default IIS site. AuthPass.aspx is set as the default page on the IIS web site
- Secure access mode is set to “Gateway Direct” but this will depend on your environment

**CSG**

- This CSG is entirely configure using the CSG Management Console
- A specific certificate for the CSG is selected
- The CSG is set to listen on specific IP address rather than the default of all IPv4 addresses (an additional address must be added to the server’s TCP config).
- “Direct” mode is configured for the Web Interface location

There you have it. Citrix WI/CSG SSO for ISA. I know it’s a bit of a hack but I spent some time trying to find a way to do this natively with Citrix Web Interface configuration and posted in the Citrix support forums without any success. If there is a more official way to do it I’d love to hear about it.

**On a side note…**

I found a long delay during login which was fixed by disabling NetBIOS over TCP/IP on the web interface server

[ Citrix Web Interface SSO for MS ISA (1069 downloads) ](https://blog.rhysgoodwin.com/download/2459/ "Version 1.0.0")</body></html>