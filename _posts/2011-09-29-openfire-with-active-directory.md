---
title: "OpenFire with Active Directory - Elastix"
date: 2011-09-29
categories: 
  - "asterisk"
  - "mysql"
  - "technical"
  - "voip"
tags: 
  - "active-directory"
  - "asterisk-chat"
  - "asterisk-dubai"
  - "asterisk-uae"
  - "cache-error"
  - "elastix-openfire"
  - "http-error-500-internal_server_error"
  - "ip-phones-dubai"
  - "openfire"
  - "openfire-with-ad"
  - "openfirepandion"
  - "pandion"
  - "requesturi-setup-setup-finished-jsp"
  - "voip-dubai"
  - "voip-uae"
  - "xmpp"
  - "xmpp-client"
---

Create database for Openfire

```
# mysql -u root -p
mysql> create database openfire;
```

Goto IM tab in Elastix GUI and start the installation.

[![](/images/1.png "1")](/images/1.png)

Select the MySQL DB which you have created already.

[![](/images/open_fire_db.png "Open_fire_db")](/images/open_fire_db.png)

Select Profile Settings as Directory Server [![](/images/5.png "5") ](/images/5.png)Put Active Directory Credentials

[![](/images/open_fire.png "Open_fire")](/images/open_fire.png) Leave User Mapping as it is ![](/images/3.png "3")

Choose the admin accounts for Open Fire.

![](/images/4.png "4")

Click Continue.

If you face the "HTTP ERROR: 500 INTERNAL\_SERVER\_ERROR" run the following commands.

```
# mkdir /var/lib/php/session/cache
# chmod 777 cache
```

Click browser's back button and click continue to finish the installation.

[![](/images/of_installed.png "OF_Installed")](/images/of_installed.png)

![](/images/pandion.png "Pandion")

For testing install XMPP client [Pandion](http://pandion.im/download) and login with Active Directory username @ Openfire Server name and active directory password. See the screenshot.
