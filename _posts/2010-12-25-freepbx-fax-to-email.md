---
title: "FreePBX - Custom FAX to email"
date: 2010-12-25
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-dubai-uae"
  - "embedded-system"
  - "fax"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "pika"
  - "voip-dubai"
---

[![](/images/fax2em.png "FAX2EM")](/images/fax2em.png) <!--more--> FreePBX already has a FAX to email solution built into it., but it is a bit difficult (at least for me) to configure that on a embedded system like [PIKA](www.pikatechnologies.com/). So I decided to use some different script to do that job for me. I reused the same send mail script which used for Digium FFA to send FAX(Find it [here](http://www.voip-info.org/wiki/view/Digium+Free+Fax+For+Asterisk+and+sendEmail)). When you are using sendEmail script you can include the smtpserver address,username & password in the same script so that those will not be shown when you use Asterisk CLI for debugging. To include those credentials change the following lines in sendEmail. Change the default SMTP Server under #Network `#Network "server" => 'smtp.yourserver.com', ## Default SMTP server`

and change the eSMTP settings also `## eSMTP "username" => 'faxmailer@yourserver.com', ## Username used in SMTP Auth "password" => 'password', ## Password used in SMTP Auth "tls" => 'auto', ## Enable or disable TLS support. Options: auto, yes, no`

To include custom dialplan in FreePBX we have to edit extensions\_custom.conf. Here I have a dedicated FAX line which is connected to the 5th FXO port. All inbound calls will be forwarded to the context \[from-pstn-custom\] from there the dialplan will decide where the call go.,here if the call is on port 5 it will be forward to fax otherwise to IVR.

`[from-pstn-custom] exten => s,1,Noop(Entering FAX Detection) exten => s,n,Ringing() exten => s,n,Set(DID=${IF($["${DID**"= ""]?s:${DID**)**) exten => s,n,Noop(DID is now ${DID**) exten => s,n,Set(CHAN=${CHANNEL:4**) exten => s,n,Set(CHAN=${CUT(CHAN,/,3)**) exten => s,n,GotoIf($["${CHAN}" = "5"]?fax:ivr) exten => s,n(fax),Goto(ext-fax-custom,s,1) exten => s,n(ivr),Goto(ivr-2,s,1) exten => fax,1,Goto(ext-fax-custom,s,1) [ext-fax-custom] exten => s,1,Noop(Receiving Fax From: ${CALLERID(all)}) exten => s,n,Answer exten => s,n,Wait(4) exten => s,n,Macro(user-callerid,) exten => s,n,Set(FAX_RX_EMAIL='user1@yourserver.com,user2@yourserver.com') exten => s,n,Set(LOCALHEADERINFO="FAX") exten => s,n,Set(LOCALSTATIONID="048847010") exten => s,n,Set(PIKAFAXMODEMTYPE=15) exten => s,n(receivefax),PIKARxFax(${ASTSPOOLDIR}/fax/${UNIQUEID}.tif) exten => s,n,Noop(Result: ${FAXRESULT}) exten => s,n,Noop(Last Error: ${FAXLASTERROR}) exten => s,n,Noop(Remote ID: ${REMOTESTATIONID}) exten => s,n,Noop(Num Pages: ${FAXPAGES}) exten => s,n,Noop(Bit Rate: ${FAXBITRATE}) exten => s,n,Noop(Resolution: ${FAXRESOLUTION}) exten => h,1,GotoIf($["${FAXSTATUS:0:6}" = "FAILED"]?failed) exten => h,n(process),GotoIf($[${LEN(${FAX_RX_EMAIL})} = 0]?end) exten => h,n,System(${ASTVARLIBDIR}/bin/sendfax -t ${FAX_RX_EMAIL} -u Received FAX from ${CALLERID(num)} -a ${ASTSPOOLDIR}/fax/${UNIQUEID}.tif -m You have received a new fax from ${CALLERID(num)}. Please find attached. -l /var/log/asterisk/sendEmail.log) exten => h,n(end),Macro(hangupcall,) exten => h,process+101(failed),Noop(FAX ${FAXSTATUS} for: ${FAX_RX_EMAIL} , From: ${CALLERID(all)}) exten => h,n,Macro(hangupcall,)`
