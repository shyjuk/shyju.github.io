---
title: "VtigerCRM Click to Dial with Elastix"
date: 2010-09-15
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "voip-dubai"
---

In elastix 2.0 the vtigerCRM click to dial normally won't work . We have to change the following file to make it work.

**Edit the following file**

_/var/www/html/vtigercrm/modules/PBXManager/utils/AsteriskClass.php_

Search for default:

change the $context = "default"; to $context = "from-internal";

(from-interna is default context for extensions in FreePBX)

where "from-internal" is the dial plan context for oubound dialing for the user extension who logged in to vtigercrm.

See _/etc/asterisk/sip\_additional.conf_ for FreePBX or _/etc/asterisk/users.conf_ for bare asterisk
