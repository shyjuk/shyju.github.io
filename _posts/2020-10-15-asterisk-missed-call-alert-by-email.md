---
title: "Asterisk - Missed call alert by email"
date: 2020-10-15
categories: 
  - "technical"
tags: 
  - "asterisk-call-alert"
  - "asterisk-dubai-uae"
  - "asterisk-misc-destination"
  - "asterisk-missed-call-alert"
  - "missed-calls"
---

- Create a Misc Destination in FreePBX.  
    

[![](/images/image-3.png)](/images/image-3.png)

- Add a new dial plan for missed call alert in /etc/asterisk/extensions\_custom.conf under \[from-internal-custom\] context  
    

[![](/images/image-4.png)](/images/image-4.png)

`exten => _*2255,1,Set(VMEMAIL=${SHELL(awk -F',' '$1 ~ /${CONNECTEDLINE(num)} / { print $3 }' /etc/asterisk/voicemail.conf | tr -d '\n')})   exten => _*2255,,n,TrySystem(echo "Call from ${CALLERID(name)} at ${CALLERID(number)} received ${STRFTIME(${EPOCH},%l:%M:%S %p %Z on %A %B %e)}" | mail -s " You missed a call from ${CALLERID(name)}: ${CALLERID(number)}" ${VMEMAIL})   exten => _*2255,n,Hangup()`

- Edit extension and add your voicemail email.  
    

[![](/images/image-6.png)](/images/image-6.png)

- Change "-Optional Destinations" to the new misc. destination and submit.  
    

[![](/images/image-5.png)](/images/image-5.png)

- Dial the extension and wait till it forward the call to misc. destination.  
    

[![](/images/image-7.png)](/images/image-7.png)

-
