---
title: "Call Forwarding from outside the office"
date: 2011-07-31
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "voip"
tags: 
  - "asterisk-and-mobile"
  - "asterisk-call-forward"
  - "asterisk-dubai-uae"
  - "call-cascade-to-mobile"
  - "call-forward-to-mobile"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "office-land-line-to-mobile"
  - "office-line-to-mobile"
  - "office-number-forward"
  - "remote-office"
  - "voip-dubai"
---

See the [previous](https://shyju.wordpress.com/2011/07/29/call-forwardingfollow-me-without-delay-ringing-sound-asterisk-1-4/) call forward setup.  Use the same setup and append the below lines to /etc/asterisk/extensions\_custom.conf , change the numbers to where you want to enable call forwarding(ie. the your mobile number from where you pick up the forwarded call)  and reload asterisk. Dial your office number from your mobile when you reach IVR dial 9 which will enable call forwarding . Now all calls to your office are forwarded to your mobile.

```
[ivr-5-custom]
exten => 9,1,NoOP(${CALLERID(num)})
exten => 9,n,GotoIf($["${CALLERID(num)}" = "0X5X8X0X2X" || "${CALLERID(num)}" = "05X9X4X8XX"]?cact)
exten => 9,n,Goto(ivr-5,s,1)
exten => 9,n(cact),Goto(from-internal,*${CALLERID(num)},1)
```
