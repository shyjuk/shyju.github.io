---
title: "Asterisk 1.4:Call forwarding(Follow Me) - With Music On Hold-MeetMe-Without delay"
date: 2011-07-29
categories: 
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-call-file"
  - "asterisk-dubai-uae"
  - "call-file"
  - "conference"
  - "forward-call-without-delay"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "ivr"
  - "meetme"
  - "voip-dubai"
---

**W**hen we use follow-me in FreePBX with our mobile number as the follow-me extension, the caller will hear the ring back sound when the PBX dials our mobile number.  If we are using analog lines to forward the calls even the Dial command with m option(Music On Hold) will not work in this situation.

Here I'm using  meet-me application asterisk call file  and some dial plan manipulation to do the task. All the calls from PSTN(analog lines) to IVR will be forwarded to mobile number. Create conference extension from FreePBX GUI ,create IVR and route the calls to conference number from IVR. Add a 3rd IVR and route the calls from that to same conference number., In third IVR you can add some voice menu like "You have received a call please dial 0 to accept the call" so that when we receive the forwarded call we can dial 0 and connect caller. The forward number will be dialed as soon as the  incoming call reaches PBX. So the call connection delay will be minimum.

First I've made a dial plan to Activate/Deactivate call forwarding. To enable/disable (toggle) call forwarding you have to dial \* followed by your mobile number from your extension. Find below the dialplan. Put this into /etc/asterisk/extension.conf under \[from-internal\] context. Here it is 10 digit number starting with a preceding 0.

```
[from-internal]
exten => _*0XXXXXXXXX,1,NoOP(${DB(AMPUSER/followme)})
exten => _*0XXXXXXXXX,n(start),Answer
exten => _*0XXXXXXXXX,n,Wait(1)
exten => _*0XXXXXXXXX,n,Macro(user-callerid,)
exten => _*0XXXXXXXXX,n,GotoIf($["${DB(AMPUSER/followme)}" = "0"]?activate)
exten => _*0XXXXXXXXX,n(deactivate),Set(DB(AMPUSER/followme)=0)
exten => _*0XXXXXXXXX,n,Playback(followme&de-activated)
exten => _*0XXXXXXXXX,n(end),Macro(hangupcall,)
exten => _*0XXXXXXXXX,n(activate),Set(DB(AMPUSER/followme)=${EXTEN:1})
exten => _*0XXXXXXXXX,n,Playback(followme&activated)
exten => _*0XXXXXXXXX,n(end),Macro(hangupcall,)
```

Create a sh script make it executable,make asterisk as its owner and save it as "/va/lib/asterisk/bin/clfwd.sh" . This file make a call to your mobile when a new call arrives.

```
#!/bin/bash
echo -e "Channel: DAHDI/16/$1\nMaxRetries: 2\nRetryTime: 60\nWaitTime: 45\nContext: ivr-7\nExtension: s" > /tmp/$1.call
chmod 777 /tmp/$1.call
mv /tmp/$1.call /var/spool/asterisk/outgoing/
```

Edit the file /etc/asterisk/extension\_additional.conf and add the below line under the context \[from-pstn-custom\]. Create the context \[ from-pstn-custom\] if it doesn't exists.

```
[from-pstn-custom]
exten => s,1,Noop(Entering FAX Detection)
exten => s,n,Ringing()
exten => s,n,Set(DID=${IF($["${DID**"= ""]?s:${DID**)**)
exten => s,n,Noop(DID is now ${DID**)
exten => s,n,Set(CHAN=${CHANNEL:6})
exten => s,n,Set(CHAN=${CUT(CHAN,-,1)})
exten => s,n,GotoIf($["${CHAN}" = "18"]?fax:ivr)
exten => s,n(fax),Goto(ext-fax-custom,s,1)
exten => s,n(ivr),NoOP(${DB(AMPUSER/followme)})
;exten => s,n,Set(CFNO = "${DB(AMPUSER/followme)}")
exten => s,n,GotoIf($["${DB(AMPUSER/followme)}" != "0"]?active)
exten => s,n,Goto(ivr-5,s,1)
exten => s,n,Hangup()
exten => s,n(active),Wait(2)
exten => s,n,ChanIsAvail(DAHDI/16)  ;checks if the line to forward the call is already busy.
exten => s,n,Set(CHANAV=${AVAILORIGCHAN})
exten => s,n,GotoIf($["${CHANAV}" = ""]?active)
exten => s,n,system(/var/lib/asterisk/bin/clfwd.sh ${DB(AMPUSER/followme)})
exten => s,n,Goto(ivr-7,s,1)
exten => s,n,Hangup()
exten => fax,1,Goto(ext-fax-custom,s,1)
```

Enable call forwarding by dialing your number with preceding \*.

Make a call to your PBX . The PBX makes a call to your mobile. Accept the call and dial 0 to connect the caller.

Find below the other configurations by FreePBX GUI.

;extension\_additional.conf

```
[ext-meetme]
include => ext-meetme-custom
exten => STARTMEETME,1,MeetMe(${MEETME_ROOMNUM},${MEETME_OPTS},${PIN})
exten => STARTMEETME,n,Hangup
exten => h,1,Hangup
exten => 696,1,Macro(user-callerid,)
exten => 696,n,Set(MEETME_ROOMNUM=696)
exten => 696,n,Set(MEETME_RECORDINGFILE=${ASTSPOOLDIR}/monitor/meetme-conf-rec-${MEETME_ROOMNUM}-${UNIQUEID})
exten => 696,n,GotoIf($["${DIALSTATUS}" = "ANSWER"]?USER)
exten => 696,n,Answer
exten => 696,n,Wait(1)
exten => 696,n(USER),Set(MEETME_OPTS=qMsr)
exten => 696,n,Goto(STARTMEETME,1)
exten => 696,hint,MeetMe:696
[ivr-5]
include => ivr-5-custom
include => from-did-direct-ivr
include => app-directory
exten => #,1,dbDel(${BLKVM_OVERRIDE})
exten => #,n,Set(__NODEST=)
exten => #,n,Goto(app-directory,#,1)
exten => h,1,Hangup
exten => s,1,Set(MSG=custom/ePillars_IVR)
exten => s,n,Set(LOOPCOUNT=0)
exten => s,n,Set(__DIR-CONTEXT=default)
exten => s,n,Set(_IVR_CONTEXT_${CONTEXT}=${IVR_CONTEXT})
exten => s,n,Set(_IVR_CONTEXT=${CONTEXT})
exten => s,n,GotoIf($["${CDR(disposition)}" = "ANSWERED"]?begin)
exten => s,n,Answer
exten => s,n,Wait(1)
exten => s,n(begin),Set(TIMEOUT(digit)=3)
exten => s,n,Set(TIMEOUT(response)=10)
exten => s,n,Set(__IVR_RETVM=)
exten => s,n,ExecIf($["${MSG}" != ""],Background,${MSG})
exten => s,n,WaitExten(,)
exten => hang,1,Playback(vm-goodbye)
exten => hang,n,Hangup
exten => 0,1,dbDel(${BLKVM_OVERRIDE})
exten => 0,n,Set(__NODEST=)
exten => 0,n,Goto(from-did-direct,603,1)
exten => 1,1,dbDel(${BLKVM_OVERRIDE})
exten => 1,n,Set(__NODEST=)
exten => 1,n,Goto(from-did-direct,603,1)
exten => 2,1,dbDel(${BLKVM_OVERRIDE})
exten => 2,n,Set(__NODEST=)
exten => 2,n,Goto(from-did-direct,603,1)
exten => 3,1,dbDel(${BLKVM_OVERRIDE})
[ivr-6]
include => ivr-6-custom
include => from-did-direct-ivr
include => app-directory
exten => #,1,dbDel(${BLKVM_OVERRIDE})
exten => #,n,Set(__NODEST=)
exten => #,n,Goto(app-directory,#,1)
exten => h,1,Hangup
exten => s,1,Set(MSG=custom/ePillars_IVR)
exten => s,n,Set(LOOPCOUNT=0)
exten => s,n,Set(__DIR-CONTEXT=default)
exten => s,n,Set(_IVR_CONTEXT_${CONTEXT}=${IVR_CONTEXT})
exten => s,n,Set(_IVR_CONTEXT=${CONTEXT})
exten => s,n,GotoIf($["${CDR(disposition)}" = "ANSWERED"]?begin)
exten => s,n,Answer
exten => s,n,Wait(1)
exten => s,n(begin),Set(TIMEOUT(digit)=3)
exten => s,n,Set(TIMEOUT(response)=10)
exten => s,n,Set(__IVR_RETVM=)
exten => s,n,ExecIf($["${MSG}" != ""],Background,${MSG})
exten => s,n,WaitExten(,)
exten => hang,1,Playback(vm-goodbye)
exten => hang,n,Hangup
exten => 0,1,dbDel(${BLKVM_OVERRIDE})
exten => 0,n,Set(__NODEST=)
exten => 0,n,Goto(ext-meetme,696,1)
exten => 1,1,dbDel(${BLKVM_OVERRIDE})
exten => 1,n,Set(__NODEST=)
exten => 1,n,Goto(ext-meetme,696,1)
exten => 2,1,dbDel(${BLKVM_OVERRIDE})
exten => 2,n,Set(__NODEST=)
exten => 2,n,Goto(ext-meetme,696,1)

[ivr-7]
include => ivr-7-custom
include => from-did-direct-ivr
include => app-directory
exten => #,1,dbDel(${BLKVM_OVERRIDE})
exten => #,n,Set(__NODEST=)
exten => #,n,Goto(app-directory,#,1)
exten => h,1,Hangup
exten => s,1,Set(MSG=custom/Call)
exten => s,n,Set(LOOPCOUNT=0)
exten => s,n,Set(__DIR-CONTEXT=default)
exten => s,n,Set(_IVR_CONTEXT_${CONTEXT}=${IVR_CONTEXT})
exten => s,n,Set(_IVR_CONTEXT=${CONTEXT})
exten => s,n,GotoIf($["${CDR(disposition)}" = "ANSWERED"]?begin)
exten => s,n,Answer
exten => s,n,Wait(1)
exten => s,n(begin),Set(TIMEOUT(digit)=3)
exten => s,n,Set(TIMEOUT(response)=3)
exten => s,n,Set(__IVR_RETVM=)
exten => s,n,ExecIf($["${MSG}" != ""],Background,${MSG})
exten => s,n,WaitExten(,)
exten => hang,1,Playback(vm-goodbye)
exten => hang,n,Hangup
exten => 0,1,dbDel(${BLKVM_OVERRIDE})
exten => 0,n,Set(__NODEST=)
exten => 0,n,Goto(ext-meetme,696,1)
exten => 1,1,dbDel(${BLKVM_OVERRIDE})
exten => 1,n,Set(__NODEST=)
exten => 1,n,Goto(ext-meetme,696,1)
exten => 2,1,dbDel(${BLKVM_OVERRIDE})
exten => 2,n,Set(__NODEST=)
exten => 2,n,Goto(ext-meetme,696,1)
```

meetme\_additional.conf

```
conf => 696,
```
