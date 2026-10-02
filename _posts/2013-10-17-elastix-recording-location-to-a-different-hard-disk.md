---
title: "Elastix Recording Location to a different Hard Disk"
date: 2013-10-17
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-recording"
  - "change-monitor-location"
  - "diy"
  - "freepbx-recording"
  - "hard-disk"
  - "pbx-aue"
  - "pbx-dubai"
  - "pbx-uae"
  - "startup-script"
---

1\. Add the new formatted Hard disk.

2\. Create a new mount point for the disk and make asterisk as the owner of that.

#mkdir /monitor

#chown -R asterisk:asterisk /monitor/

3\. Create startup script. You can also use fstab for the same.

Edit /etc/rc.local and add the following line. Here  /dev/sdb1 is the first partition of newly attached hard disk.

/bin/mount /dev/sdb1 /monitor

4\. Run the startup script.

#cd /etc

#./rc.local

5\. Now if you run the below command you should see the hard disk mounted( /dev/sdb1             903G  2.9G  853G   1% /monitor). # df -h

Filesystem            Size  Used Avail Use% Mounted on

/dev/sda3             895G  2.8G  846G   1% /

/dev/sda1             487M   23M  439M   5% /boot

tmpfs                 2.0G     0  2.0G   0% /dev/shm

/dev/sdb1             903G  2.9G  853G   1% /monitor

6\. Remove the monitor folder and link it to new location.

#rm -rf /var/spool/asterisk/monitor

#/bin/ln -s /monitor /var/spool/asterisk

#chown asterisk /var/spool/asterisk/monitor

7\. Change Extension settings to enable recording in Elastix GUI and check if the files are saved in new location.
