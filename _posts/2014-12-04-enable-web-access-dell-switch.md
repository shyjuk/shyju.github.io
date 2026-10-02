---
title: "Enable Web Access - Dell Switch"
date: 2014-12-04
categories: 
  - "technical"
tags: 
  - "6224"
  - "dell-power-connect"
---

1.  Connect with RS232 console cable.
2.  Change putty settings. Change the serial line connection numbers if it does not connect(COM1,COM2 etc.)

[![Dell-Switch](/images/dell-switch.png)](/images/dell-switch.png)

 

 

 

 

 

 

 

 

 

 

 

 

3\. Enter the following commands and access the interface using

IP:  192.168.1.254. Username/password : admin/mypass

 

en config ip address 192.168.1.254 255.255.255.0 ip default-gateway 192.168.1.1 username admin password mypass level 15
