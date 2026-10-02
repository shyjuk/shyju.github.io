---
title: "Elastix Yealink Provisioning  Add BLF Configuration"
date: 2013-12-14
categories: 
  - "technical"
tags: 
  - "blf"
  - "elastix"
  - "enable"
  - "extension-asc"
  - "ip-phones-uae"
  - "telephone-system-uae"
  - "yealink-dubai"
  - "yealink-provisioning"
  - "yealink-uae"
---

Edit /var/www/html/modules/endpoint\_configurator/libs/vendors/Yealink.cfg.php

**Enable BLF Button**

Add the following function at the end of the file

Change the password in 2nd line of the code according to your configuration.

\----------------------------------------------------------------------------------------------------------------------------------------

**function addBLF($dev\_id){**

**$mysql\_conn = mysql\_connect('localhost', 'root', 'password');**

**mysql\_select\_db('asterisk', $mysql\_conn );**

**$result= mysql\_query("select extension from users where extension!=$dev\_id order by extension asc", $mysql\_conn);**

**$ki=1;**

**$content="";**

**while($row = mysql\_fetch\_row($result))**

**{**

**$pcnt = "**

**\[ memory".$ki." \]**

**path = /yealink/config/vpPhone/vpPhone.ini**

**Line = 0**

**type = blf**

**Value = $row\[0\]**

**Callpickup =**

**DKtype = 16**

**PickupValue = \*\***

**";**

**$ki++;**

**$content=$content.$pcnt;**

**}**

**return $content;**

**}**

\----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Add the below line before return $content.

_\[ memory16 \]_

_path = /yealink/config/vpPhone/vpPhone.ini_

_DKtype = 15_

_Line = 1_

_Value =_

_type =_

_";_

_**$content=$content.addBLF($id\_device);**_

 _return $content;_
