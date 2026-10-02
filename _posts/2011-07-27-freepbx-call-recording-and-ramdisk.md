---
title: "FreePBX - Call Recording and RAMDISK"
date: 2011-07-27
categories: 
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-call-record-directory"
  - "asterisk-dubai-uae"
  - "asterisk-recording"
  - "call-recording"
  - "change-asterisk-record-location"
  - "elastix-call-record-directory"
  - "elastix-call-recording"
  - "elastix-monitor-record-location"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "ram-disk"
  - "ramdisk"
  - "voip-dubai"
---

**A**sterisk call recording is resource intensive especially when the number of calls in the PBX is high. If you record all the calls directly to the HDD in asterisk  pbx and you got a large call volume (number of calls) then it will damage your PBX's HDD very soon. On such cases using RAMDISK and seperate HDD may solve the problem.

Install freepbx. If you are not using SAN  use 2 HDDs for better results. One for OS+asterisk and one as a storage for recorded files.  Here I'm using /record as the storage location and  /astrec as the mount point for RAMDISK. Format and mount the 2nd HDD  to /record at the time of OS installation(or create an fstab entry if you are doing it later). After installation give write permission or assign asterisk user as the owner of /record & /astrec.

```
/bin/chown -R asterisk.asterisk /record
/bin/mkdir  /astrec
/bin/chown -R asterisk.asterisk /astrec
```

After the freepbx installation edit "/boot/grub/grub.conf" and put the the ramdisk size.

```
#boot=/dev/hda
default=0
timeout=5
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu
title Elastix (2.6.18-194.3.1.el5)
        root (hd0,0)
        kernel /vmlinuz-2.6.18-194.3.1.el5 ro root=LABEL=/
ramdisk_size=1000000
initrd /initrd-2.6.18-194.3.1.el5.img
```

Here I used RAMDISK size as 1GB(1000000) . The size depends on the number of calls,the average time for one call,and the recording format.

Remove the 3 directories from asterisk spool directory because we are going to use ramdisk instead of those.

```
rm -rf /var/spool/asterisk/monitor
rm -rf /var/spool/asterisk/system
rm -rf /var/spool/asterisk/tmp
```

Edit the file "_/etc/rc.local"_ and append the  below code.

```
#Put this in rc.local to create ramdisk during bootup
/sbin/mke2fs -m 0 /dev/ram0
/bin/mount /dev/ram0 /astrec
/bin/mkdir /astrec/monitor
/bin/mkdir /astrec/system
/bin/mkdir /astrec/tmp
/bin/chown -R asterisk.asterisk /astrec
#For more infor on ramdisk:
#http://www.vanemery.com/Linux/Ramdisk/ramdisk.html

#Link asterisk's default folders to our ramdisk
/bin/ln -s /astrec/system /var/spool/asterisk
/bin/ln -s /astrec/tmp /var/spool/asterisk
/bin/ln -s /astrec/monitor /var/spool/asterisk
```

At the time of booting the above lines in rc.local creates RAMDISK and mount it to "/astrec".

Create the file "_/var/lib/asterisk/agi-bin/mvrec.sh_" and put the below contents.

```
#!/bin/bash
/bin/mv $1/$2.$3 /record 3>&1 1>&2 2>&3 | tee /var/log/asterisk/mvrec.log
```

The above code will transfer the recorded file from RAMDISK to the location /record., Use the file "_/var/log/asterisk/mvrec.log"_  for troubleshooting purpose. Make the script executable.

```
chmod 755 /var/lib/asterisk/agi-bin/mvrec.sh
```

Log in to freepbx interface and change the General Settings as shown in screen shot.

```
Extension Recording Override:Enabled
Call recording format:	wav
Recording Location:	/var/spool/asterisk/monitor/
Run after record: /var/lib/asterisk/agi-bin/mvrec.sh ^{MIXMON_DIR} ^{CALLFILENAME} ^{MIXMON_FORMAT}
```

[![](/images/mvrec.png "mvrec")](/images/mvrec.png)

On FreePBX GUI goto  "Extensions" and select one of the extensions in which you want to enable call recording.

If you are using Elastix change the recording directory in elastix monitoring configuration file.

```
 /var/www/html/modules/monitoring/configs/default.conf.php
 $arrConfModule['records_dir']       = '/record/';
```

Select the appropriate recording option.

[![](/images/recalways.png "recalways")](/images/recalways.png)

Try calling between 2 extension after enabling call recording.  See the asterisk cli output. Use the file "/var/log/asterisk/mvrec.log" for troubleshooting.

[![](/images/output.png "output")](/images/output.png)If you are using specific context other than from-internnal context, don't forget to add the Macros. For example

```
[custom-checklock]
exten => _.,1,NoOp(AMPUSER/${CALLERID(num)}/locked)
exten => _.,n,Gotoif($[${DB_EXISTS(AMPUSER/${CALLERID(num)}/locked)}=0]?carryon)
exten => _.,n,Set(PINCOUNT=0)
;exten => _.,n,Set(VALIDPIN=1234)
exten => _.,n(readpin),Read(PIN,vm-password,4,,,4)
exten => _.,n,Gotoif($[${PIN}=${VALIDPIN}]?carryon)
exten => _.,n,Gotoif($[${PIN}=${DB(AMPUSER/${CALLERID(num)}/locked)}]?carryon)
exten => _.,n,Set(PINCOUNT=$[${PINCOUNT}+1])
exten => _.,n,Playback(vm-incorrect)
exten => _.,n,GotoIf($[${PINCOUNT}>2]?h)
exten => _.,n,Wait(1)
exten => _.,n,Goto(readpin)
exten => _.,n(carryon),Set(DB(AMPUSER/${CALLERID(num)}/unlocked)=${PIN})
exten => _.,n,NoOp(${DB_DELETE(AMPUSER/${CALLERID(num)}/locked)})
;exten => _.,n,Dial(DAHDI/g0/${EXTEN})
exten => _.,n,Macro(user-callerid,SKIPTTL,)
exten => _.,n,Macro(record-enable,${AMPUSER},OUT,)
exten => _.,n,Macro(dialout-trunk,2,${EXTEN},)
exten => h,1,Macro(hangupcall,)
;exten => 911,1,Goto(CallingRule_Out39,${EXTEN},1)
;exten => h,1,Hangup

[custom-setlock]
exten => s,1,NoOp(In Set Lock)
exten => s,n,Gotoif($[${DB_EXISTS(AMPUSER/${CALLERID(num)}/unlocked)}=1]?lockit)
exten => s,n,Gotoif($[${DB_EXISTS(AMPUSER/${CALLERID(num)}/locked)}=0]?readit)
exten => s,n,Read(LOCKEY,vm-password,4,,,4)
exten => s,n,Gotoif($[${LOCKEY}=${DB(AMPUSER/${CALLERID(num)}/locked)}]?readit)
exten => s,n,Playback(vm-pls-try-again)
exten => s,n,Hangup
exten => s,n(readit),Read(PIN1,vm-newpassword,4,,,4)
exten => s,n,Read(PIN2,vm-reenterpassword,4,,,4)
exten => s,n,Gotoif($[${PIN1}=${PIN2}]?okey)
exten => s,n,Playback(an-error-has-occured)
exten => s,n,Hangup
exten => s,n(okey),Set(DB(AMPUSER/${CALLERID(num)}/locked)=${PIN1})
exten => s,n,Playback(locked)
exten => s,n,Hangup
exten => s,n(lockit),Set(DB(AMPUSER/${CALLERID(num)}/locked)=${DB(AMPUSER/${CALLERID(num)}/unlocked)})
exten => s,n,NoOp(${DB_DELETE(AMPUSER/${CALLERID(num)}/unlocked)})
exten => s,n,Playback(locked)
exten => h,1,Hangup

[national]
include => from-internal
exten => _XXXXXXXXX,1,Goto(custom-checklock,${EXTEN},1)

[international]
include => from-internal
exten => _XXXX.,1,NoOp(AMPUSER/${CALLERID(num)}/locked)
exten => _XXXX.,n,Goto(custom-checklock,${EXTEN},1)

[mobile]
include => from-internal
exten => 123,1,Goto(custom-setlock,s,1)
exten => _XXXXXXXXX,1,Goto(custom-checklock,${EXTEN},1)
exten => _XXXXXXXXXX,1,Goto(custom-checklock,${EXTEN},1)

[changepw]
exten => s,1,Read(LOCKEY,vm-password,4,,,4)
exten => s,n,Gotoif($[${LOCKEY}=2008]?okey)
exten => s,n,Hangup
exten => s,n(okey),Read(EXTNO,superlock,3,,,4)
exten => s,n,Set(DB(AMPUSER/${EXTNO}/locked)=1234)
exten => s,n,Playback(locked)
exten => h,1,Hangup
```
