---
title: "OpenVZ Container - Allow access from only one IP and disable all outbound traffic"
date: 2023-07-05
categories: 
  - "technical"
---

Edit the container configuration and add the config

```
vi /etc/vz/conf/1123.conf 
```

```
IPTABLES="iptable_filter iptable_mangle ipt_limit ipt_multiport ipt_tos ipt_TOS ipt_REJECT ipt_TCPMSS ipt_tcpmss ipt_ttl ipt_LOG ipt_length ip_conntrack ip_conntrack_ftp ip_conntrack_irc ipt_conntrack ipt_state  ipt_helper  iptable_nat ip_nat_ftp ip_nat_irc ipt_REDIRECT"
```

Login to container and add IPtable rules for allow access from only one IP

```
iptables -A INPUT -s 192.168.160.1 -j ACCEPT
iptables -A OUTPUT -d 192.168.160.1 -j ACCEPT
iptables -P INPUT DROP
iptables -P OUTPUT DROP
```
