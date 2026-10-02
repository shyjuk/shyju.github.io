---
title: "Yealink VLAN DHCP Scope configuration on VLAN Windows DHCP Server"
date: 2017-02-15
categories: 
  - "technical"
tags: 
  - "asterisk"
  - "boot-server"
  - "dhcp-vlan"
  - "vlan-over-dhcp"
  - "windows"
  - "windows-dhcp"
  - "yealink-boot"
  - "yealink-vlan"
---

## Yealink VLAN DHCP Scope configuration on VLAN Windows DHCP Server

To configure Yealink phones to get VLAN ID from the default DHCP Server follow the below instructions.

### Step 1

Login to local windows dhcp server and configure a new scope option for Yealink phone. a) Right click IPv4 b) select "Set Predefined Options" [![windows\_dhcp\_define\_new\_scope\_option](/images/windows_dhcp_define_new_scope_option1.png)](/images/windows_dhcp_define_new_scope_option1.png)

### Step 2

a) Click on "Add" b) Fill the "Option Type" c) Select "Data Type" as String. d) Code is "132" e) Finish the Options by clicking "OK" on all other dialog boxes. [![yealink\_define\_vlan\_code](/images/yealink_define_vlan_code.png)](/images/yealink_define_vlan_code.png)

### Step 3

a) Right click Scope Options b) Select "Configure Options" [![configure\_dhcp\_options](/images/configure_dhcp_options1.png)](/images/configure_dhcp_options1.png)

### Step 4

a) Scroll till the end of the list and select the Yealink VLAN option. b) Under String value put the VLAN ID which you want to set on yealink phone.

[![yealink\_dhcp\_vlan\_scope](/images/yealink_dhcp_vlan_scope.png)](/images/yealink_dhcp_vlan_scope.png)
