---
title: "EBS HTTP Error"
date: 2011-12-15
categories: 
  - "technical"
tags: 
  - "http-listener-is-not-responding-oracle"
  - "http-listner-error-oracle"
  - "oracle-apps-http-error"
  - "oracle-ebs"
---

Our website has a new address. You can now find us at https://www.shyju.in. We hope you will continue to visit us!

http listener is not responding.  
If you find this error after Oracel EBS installation, you may be missing one entry in /etc/hosts file. Add the following line and restart network and click retry on "Install Oracle Application's - Post Install dialog.

127.0.0.1 localhost.localdomain localhost

#/etc/init.d/network restart
