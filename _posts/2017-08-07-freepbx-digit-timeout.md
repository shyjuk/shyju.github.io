---
title: "FreePBX Digit Timeout"
date: 2017-08-07
categories: 
  - "technical"
tags: 
  - "asterisk-timeout"
  - "freepbx"
  - "timeout"
---

Edit the file "/var/www/html/admin/modules/ivr/functions.inc.php"

Change the ext\_digittimeout(3) ext\_digittimeout(1) for 1 second dial delay.

$ext->add($c, 's', 'start', new ext\_digittimeout(1));
