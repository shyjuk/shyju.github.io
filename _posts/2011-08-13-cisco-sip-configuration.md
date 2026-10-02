---
title: "Cisco SIP Configuration"
date: 2011-08-13
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "mysql"
  - "technical"
  - "voip"
tags: 
  - "asterisk-dubai"
  - "asterisk-uae"
  - "cisco-7911g"
  - "cisco-7945-sip"
  - "cisco-ip-phone"
  - "cisco-ip-phone-sip-configuration"
  - "cisco-sip-firmware"
  - "cisco-tftp-sip"
  - "cisco-with-asterisk"
  - "ip-phones-dubai"
  - "voip-dubai"
  - "voip-uae"
---

### Cisco 7911G/7942/7945/7962 Phone with Asterisk

Download the firmware ([7911](http://dl.dropbox.com/u/10036721/cmterm-7911_7906-sip.8-2-2SR4.zip) ,[7942](http://dl.dropbox.com/u/10036721/Cisco-7942-SIP.zip "7942"), [7945](http://dl.dropbox.com/u/10036721/Cisco-7945-SIP.zip) , [7962](http://dl.dropbox.com/u/10036721/Cisco-7962-SIP.zip)) and extract it.

Download and install/extract the [tftp server](http://tftpd32.jounin.net/download/tftpd32.400.zip) software.

Open the tftp server software and make the SIP firmware  extracted directory as the root directory of the tftp server.

Goto command prompt(Start>Run>CMD and press enter) and enter the following command.

C:\\Users\\user>tftp <tftp-server-ip-address> get dialplan.xml

You should get the message starting "Transfer Successful".(If your OS is Win7/Vista you have to install tftp client from the Add/Remove Programs)

```
C:\Users\shyju>tftp 192.168.20.124 get dialplan.xml
Transfer successful: 258 bytes in 1 second(s), 258 bytes/s
```

Open your dhcp server configuration and add  TFTP server IP address as the boot server in DHCP scope Options. Refer [this article](http://shyju.wordpress.com/2011/02/04/provision-polycom-with-freepbx/) to configure DHCP Options.

Rename the  with SEP<MAC-ADDRESS-OF-YOUR-PHONE>.cnf.xml. Then open that file and change the following lines to match with your IP PBX details.

```
<processNodeName>
<featureLabel>
<proxy>
<port>
<name>
<displayName>
<authName>
<authPassword>
```

Edit your Asterisk SIP configuration and add nat = no below the user context.

This step is important otherwise the phones will not register and on the phone's display you can see the message Registering..

If you are using FreePBX the file will be /etc/asterisk/sip\_additional.conf, In the case of Asterisk-GUI file is /etc/asterisk/users.conf

```
[610]
deny=0.0.0.0/0.0.0.0
type=friend
secret=jbsdf7h4ks
qualify=yes
port=5060
pickupgroup=
permit=0.0.0.0/0.0.0.0
nat=no
mailbox=610@device
host=dynamic
dtmfmode=rfc2833
dial=SIP/610
context=from-internal
canreinvite=no
callgroup=
callerid=device <610>
allow=all
accountcode=
call-limit=50
```
