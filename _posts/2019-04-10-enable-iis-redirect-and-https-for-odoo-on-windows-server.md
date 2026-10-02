---
title: "Enable IIS redirect and https for odoo on Windows server"
date: 2019-04-10
categories: 
  - "technical"
---

I'm slowly migrating to new site. Find the new URL for this post. >>  
https://www.shyju.in/posts/ENABLE-HTTPS-ON-WINDOWS-FOR-ODOO-ERP/

****Install below IIS**** **dependencies**

[Microsoft Application Request Routing 3.0 (x64)](https://www.microsoft.com/en-us/download/details.aspx?id=47333)  
[URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)

IIS certificate installation & configuration is not covered here.

Enable Proxy

Open IIS Management console >> application request routing

[![](/images/image.png)](/images/image.png)

Open proxy settings  

[![](/images/image-1.png)](/images/image-1.png)

Enable Proxy  

[![](/images/image-2.png)](/images/image-2.png)

Add Reverse proxy rules

Open URL Rewrite. This icon is present at the level or each site and web-application you have in the server, and will allow you to configure re-write rules that will apply from that level downwards.  

[![](/images/image-3.png)](/images/image-3.png)

**Setup a Reverse Proxy rule using the Wizard.  
**

Open the IIS Manager Console and click on the Default Web Site from the tree view on the left. Select the URL Rewrite Icon from the middle pane, and then double click it to load the URL Rewrite interface.

Chose the 'Add Rule' action from the right pane of the management console, and the select the 'Reverse Proxy Rule' from the 'Inbound and Outbound Rules' category.

Now we can proceed to fill in the routing information based on the diagram above in the Wizard window that is provided to us.

Please make sure odoo is accessible from the same server http://localhost:8069 or change the below as per your odoo URL.

![](/images/image.png)

If you want to enable redirect from http to https follow the below URL and make sure to put it as the first rule. ( Use "Move up" arrow after you write the redirect rule)

https://blogs.technet.microsoft.com/dawiese/2016/06/07/redirect-from-http-to-https-using-the-iis-url-rewrite-module/

Find below my web.config file.

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
<system.webServer>
<rewrite>
<rules>
<clear />
<rule name="ERP_http_https" patternSyntax="Wildcard" stopProcessing="true">
<match url="*" />
<conditions logicalGrouping="MatchAny" trackAllCaptures="false">
<add input="{HTTPS}" pattern="off" />
</conditions>
<action type="Redirect" url="https://{HTTP_HOST}{REQUEST_URI}" redirectType="Temporary" />
</rule>
<rule name="ReverseProxyInboundRulel" stopProcessing="true">
<match url="(.*)" />
<conditions logicalGrouping="MatchAll" trackAllCaptures="false" />
<action type="Rewrite" url="http://127.0.0.1:8069/{R:1}" />
</rule>
</rules>
<outboundRules>
<rule name="ReverseProxyOutboundRulel" preCondition="ResponseIsHtml1">
<match filterByTags="A, Form, Img" pattern="http(s)?://127.0.0.1:8069/(.*)" />
<action type="Rewrite" value="http{R:1}://myserver.com/{R:2}" />
</rule>
<preConditions>
<preCondition name="ResponseIsHtml1">
<add input="{RESPONSE_CONTENT_TYPE}" pattern="^text/html" />
</preCondition>
</preConditions>
</outboundRules>
</rewrite>
<httpRedirect enabled="false" destination="https://myserver.com" exactDestination="false" childOnly="true" />
</system.webServer>
</configuration>
```

![](/images/image-1.png)
