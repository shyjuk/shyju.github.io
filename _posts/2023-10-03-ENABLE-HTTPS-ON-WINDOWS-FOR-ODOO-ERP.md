---
title: ENABLE HTTPS ON WINDOWS FOR ODOO ERP 
date: 2023-10-03 
categories: [Windows, IIS]
tags: [odoo,windows,iis,redirect,proxy]     # TAG names should always be lowercase
---

# Securing Your Odoo ERP: Step-by-Step Guide to Enabling HTTPS with IIS on Windows

Odoo is an open-source business management software suite that includes a wide range of applications and modules for various business needs. It is designed to help organizations manage their operations more efficiently and effectively.

Odoo primarily recommends using Linux as the preferred operating system for installation, although there may be unusual cases where specific requirements necessitate its installation on a Windows OS. In this setup, I am utilizing Windows' built-in web server, IIS, as the proxy server for Odoo.

Installation of IIS is not discussed in this guide. If you are unfamiliar with the process, please refer to the YouTube video provided below.
[How to Setup or Configure IIS](https://www.youtube.com/watch?v=eeBm2H1Yuok).

## Install below IIS dependencies

Please download and install following IIS dependencies from Microsoft website. 

* [Microsoft Application Request Routing 3.0 (x64)](https://www.microsoft.com/en-us/download/details.aspx?id=47333)
* [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)

## Enable Proxy

* Open IIS Management console >> application request routing
![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/7094dc70-3514-4401-93b2-4d7fbdaaab73)
_Application request routing_

* Open proxy settings
![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/15d67f1b-4571-44bc-815a-a2f83e6a3d53)
  _Proxy Settings_

* Enable Proxy
![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/009be112-389c-452c-8d95-e18eb8012100)
_Enable Proxy_

## Add Reverse proxy rules

Open URL Rewrite. This icon is present at the level or each site and web-application you have in the server, and will allow you to configure re-write rules that will apply from that level downwards.

![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/c2846aaa-62f1-4089-9af2-9615b7a78371)

## Setup a Reverse Proxy rule using the Wizard.

Open the IIS Manager Console and click on the Default Web Site from the tree view on the left. Select the URL Rewrite Icon from the middle pane, and then double click it to load the URL Rewrite interface.

Chose the ‘Add Rule’ action from the right pane of the management console, and the select the ‘Reverse Proxy Rule’ from the ‘Inbound and Outbound Rules’ category.

![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/87eab57f-01d7-4f27-b8f9-d13c94d358a2)

Now we can proceed to fill in the routing information based on the diagram above in the Wizard window that is provided to us.

Please make sure odoo is accessible from the same server http://localhost:8069 or change the below as per your odoo URL.


You can find further information regarding the redirection from HTTP to HTTPS in the article below. Ensure that you prioritize this rule by placing it as the initial one. After creating the redirect rule, use the "Move up" arrow to position it correctly

[Redirect from HTTP to HTTPS using the IIS URL Rewrite module](https://blogs.technet.microsoft.com/dawiese/2016/06/07/redirect-from-http-to-https-using-the-iis-url-rewrite-module/).

## My web.config file.
{% raw %}
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
{% endraw %}

![image](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/ee90e347-3114-4b9d-83dd-8a0080f72384)



