---
title: "How to reduce the  incoming PSTN ring delay in Asterisk."
date: 2011-06-25
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-call-delay"
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "incoming-call-delay"
  - "ip-phones-dubai"
  - "pstn-ring-detect"
  - "reduce-ring-delay"
  - "ring-delay"
  - "voip-dubai"
---

Edit /etc/asterisk/chan\_dahdi.conf and change following lines under channels context.

```
[channels]
cidstart=ring
immediate=yes
faxdetect=no
usecallerid=no ; If you are not using caller id facility.
```
