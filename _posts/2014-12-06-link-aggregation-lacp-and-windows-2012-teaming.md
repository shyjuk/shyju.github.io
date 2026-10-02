---
title: "Link Aggregation (LACP) and Windows 2012 Teaming"
date: 2014-12-06
categories: 
  - "technical"
---

### Create a vlan for iSCSI and tag ports

console>en

console#config

console(config)# interface range ethernet 1/g11-1/g12

console(config-if)# channel-group 1 mode auto

console(config-if)# exit

console(config)# interface port-channel 1

console(config-if)# switchport mode general

console(config-if)# switchport general allowed vlan add 20 tagged

console(config-if)# switchport general pvid 1

Grouped switch port 11 & port 12 & added vlan 20

![Dell-VLAN](/images/dell-vlan.png)

![Dell-VLAN-Set-General](/images/dell-vlan-set-general.png)

Add new team interface in  Windows 2012 and set Teaming mode to LACP.

[![Win2k12](/images/win2k12.png)](/images/win2k12.png)

Add another team interface for the second vlan

[![New-VLAN](/images/new-vlan.png)](/images/new-vlan.png)
