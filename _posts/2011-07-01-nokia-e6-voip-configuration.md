---
title: "Nokia E6 VOIP Configuration"
date: 2011-07-01
categories: 
  - "asterisk"
  - "asterisk"
  - "voip"
tags: 
  - "actionvoip"
  - "anna-os"
  - "asterisk-dubai-uae"
  - "e7"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "mobile-voip-uae"
  - "nokia-e6-voip"
  - "nokia-voip"
  - "nokia-voip-settings"
  - "u-a-e-voip"
  - "uae-voip-settings"
  - "voip-dubai"
  - "voip-uae"
---

### **Nokia E6 with ACTIONVOIP**

Here I brifely describes how to configure nokia E6 with a voip provider.  I could successfully configure  actionvoip with E6.

On your phone and goto Menu -> Settings->connectivity->Admin. settings ->  SIP settings

From the options select  " New sip profile"

Profile Name: shyju's VOIP

_Put just a name as you wish._

Service profile : IETF

Default destination: None

Access point in use : LINKSYS

_Use this to select your access point SSID which your are connected(Your router's Wireless connection's name)._

Public username:  sip:shyju@sip.actionvoip.com

_Use your username@yoursipserveraddress(No need to put "sip: " it will  pre append automatically)_

Use compression: No

Registration: Always on

Use security: No

**Proxy Server**

Proxy server address: sip.actionvoip.com

Realm: None

Username: shyju

_Your VOIP username_

Password:password

_Your VOIP password._

Allow loose routing: Yes

Transport type: UDP

Port : 5060

**Registrar Server**

Registrar server address: sip.actionvoip.com

Realm: None

Username: shyju

_Your voip username._

Password: password

_Your voip password._

Allow loose routing: Yes

Transport type: UDP

Port : 5060

_If your all settings are OK. , then it will show the SIP profile "Registered"._
