---
title: "Grandstream Gateway With Asterisk+Asterisk-GUI"
date: 2010-09-06
categories: 
  - "technical"
tags: 
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "voip-dubai"
---

Here I'm using Grnadstream FXO Gateway GXW4104 Login to GXW4104 Select Profile 1 SIP Server: IP address of Asterisk box SIP Registration: No

Select FXO Lines 4. Unconditional Call Forward to VOIP: ch1-4:604;

Here 604 is the extension which the incoming call to be routed in asterisk.

In Asterisk , edit the following files.

**users.conf**

```
[gxw4104]
trunkname = Grandstream 
trunkstyle = voip 
type = peer 
context = DLPN_incoming 
host = IP address of Grnadstream Gateway 
hasexten = no 
hassip = yes 
insecure = port 
canreinvite = no 
allow = all

```

**extensions.conf** 

```

[CallingRule_Call_out]
exten = _X.,1,Macro(trunkdial-failover-0.3,SIP/gxw4104/${EXTEN})

[DLPN_ePillars] 
include = CallingRule_Call_out  

[DLPN_incoming] 
exten = 604,1,Dial(SIP/604)`

```
It will be available in Asterisk-GUI after reloading asterisk. ![Asterisk-GUI](/images/gs_agui.png)
