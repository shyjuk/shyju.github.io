---
title: "SIP Trunk 3CX with FreePBX/Issabel"
date: 2022-05-19
categories: 
  - "technical"
---

3CX,FreePBX,Issabel,Elastix - All are SIP based PBX solutions. In my case Issabel is my Main PBX and 3CX is the secondary one and I have only extensions configured in 3CX. My Main PBX is connected to outside world through a PSTN gateway. I don't have any outbound route in 3CX.

Start with creating SIP trunk from 3CX side.

1. 3CX >> SIP trunk>> Add SIP Trunk  
    Use the default one, add 3CXPBX or any number as Main trunk No.

[![](/images/image.png)](/images/image.png)

a. Add the Issabel IP and other details.  

[![](/images/image-3.png)](/images/image-3.png)

b. Add DID so that callers from the Main PBX can diectly call the 3CX extentions.

[![](/images/image-4.png)](/images/image-4.png)

c. Set Inbound Parameter CallerNum and CallerName parameters.

[![](/images/image-6.png)](/images/image-6.png)

d. Change Outbound parameters to match your requirement.

[![](/images/image-7.png)](/images/image-7.png)

e. Under 3CX>> Users>> user General tab add the outbound caller ID and map the DID which we have added step b to each users.  

[![](/images/image-9.png)](/images/image-9.png)

[![](/images/image-8.png)](/images/image-8.png)

f. Create an Outbound rule to dial the Main PBX extensions from 3CX.  

[![](/images/image-15.png)](/images/image-15.png)

**Issabel / FreePBX configuration**

a. Navigate to PBX >> PBX Configuration menu. Under Trunks click on Add trunk to add a new trunk for 3CX.

b. Give a name to trunk and add the 3CX trunk details.

Trunk Name : 10000

PEER Details

```
host=dynamic
secret=3cxpwd
sendrpid=yes
type=friend
qualify=yes
```

Register String

```
10000:3cxpwd@192.168.1.3:5060/10000
```

[![](/images/image-10.png)](/images/image-10.png)

  

c. Navigate to Outbound Routes and add new route to call route the calls to 3CX extensions.  

[![](/images/image-11.png)](/images/image-11.png)

d. Apply configurations.

[![](/images/image-12.png)](/images/image-12.png)

e. Run "sip show peers" command from PBX>>Tools>Asterisk-Cli and check the Status. It should show "OK" .  

[![](/images/image-14.png)](/images/image-14.png)

3\. Login to 3CX again and check if the trunk is active. If trunk is not green check the SIP credentials , IPs, connectivity.

[![](/images/image-13.png)](/images/image-13.png)

Try calling extensions from 3CX to Main PBX and viseversa. You should be able to dial the numbers.

3CX can be configured to dial PSTN numbers via Main PBX, change the Main PBX dial plan to achive it.
