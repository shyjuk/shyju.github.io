---
title: "OpenERP : Change default web Port"
date: 2013-05-08
categories: 
  - "technical"
tags: 
  - "openerp"
  - "openerp-default-port"
  - "webserver"
---

To Change the default http port of OpenERP from 8069 to 80 in CentOS.

Add the following line in file "/etc/sysconfig/iptables" ( See the screenshot).

\-A INPUT -p tcp -m state --state NEW -m tcp --dport 8069 -j ACCEPT[![Image](/images/port.png)](/images/port.png)

Run the command. Change eth0 according to your network.

```
/sbin/iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8069 -i eth0
```

Run the command to pertinently save the rule to iptables.

```bash
iptables-save > /etc/sysconfig/iptables
```
