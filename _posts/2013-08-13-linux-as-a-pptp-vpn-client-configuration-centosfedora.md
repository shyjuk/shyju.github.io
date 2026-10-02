---
title: "Linux as a PPTP VPN Client - Configuration - CentOS,Fedora"
date: 2013-08-13
categories: 
  - "technical"
tags: 
  - "centos-vpn-client"
  - "fedora-vpn"
  - "linux-vpn-client"
  - "pptp-vpn"
  - "vpn"
---

- #### Install the required PPTP Packages.
    

**\# rpm -Uvh pptp-1.7.2-17.fc19.x86\_64.rpm pptp-setup-1.7.2-17.fc19.x86\_64.rpm**

- #### Insert the username & password in /etc/ppp/chap-secrets
    

**\# vi /etc/ppp/chap-secrets**

```
# Secrets for authentication using CHAP
# client server secret IP addresses
user1 PPTP password123 *
```

- #### Create VPN Profile
    

**\# vi /etc/ppp/peers/vpnoffice**

```
pty "pptp myoffice1.dtdns.net --nolaunchpppd"
name user1
remotename PPTP
require-mppe-128
file /etc/ppp/options.pptp
ipparam vpnoffice

```

- #### **Connect VPN using command pppd**
    

```
# pppd call vpnoffice
```

- #### Trace the logs
    

**\# tail -f /var/log/messages**

```
Aug 14 11:05:39 localhost pppd[7957]: CHAP authentication succeeded
Aug 14 11:05:39 localhost kernel: [69644.839818] PPP MPPE Compression module registered
Aug 14 11:05:39 localhost pppd[7957]: MPPE 128-bit stateless compression enabled
Aug 14 11:05:39 localhost pppd[7957]: local IP address 192.168.1.205
Aug 14 11:05:39 localhost pppd[7957]: remote IP address 192.168.1.1
```

- #### Add route to send all traffic through PPTP VPN
    

 **# sudo route add default dev ppp0**
