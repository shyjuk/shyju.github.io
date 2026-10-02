---
title: "Provision Polycom Phones with FreePBX"
date: 2011-02-04
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "add-users"
  - "asterisk-dubai-uae"
  - "directory-provisioning"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "polycom"
  - "polycom-could-not-contact-boot-server-using-existing-configuration"
  - "polycom-ftp-provisioning"
  - "provisioning"
  - "tftp"
  - "tftp-provisioning"
  - "voip-dubai"
---

**Change Polycom Addressbook when you change Display Name from Extensions in Elastix : FreePBX**

Edit the file /var/www/html/admin/modules/core/functions.inc.php

Add the following lines after line number 4477

```
$newname=$vars['name'];
//list($fn, $ln) = explode(' ',$newname);
$userparams = core_users_get($extension);
$oldname =$userparams['name'];
if (strcmp($newname, $oldname) !== 0) {
exec("sed -i -- 's/$oldname/$newname/g'    /tftpboot/polycom/contacts/*.xml"); }
```

**Prerequisites**

FreePBX TFTP Server Package DHCP Server with Bootserver enabled FTP(vsftpd) for (In new Polycom phones FTP is the default protocol)

### Install TFTP Server in PBX

#yum install tftp-server

### Change the Owner of TFTP directory to asterisk

#chown asterisk /tftpboot

Edit /etc/xinetd.d/tftp

change the line

disable                 = no

restart the xinetd service # /etc/init.d/xinetd restart Check if you can access the files from the tftp server.

#echo  "test line" > /tftpboot/test.txt

#### From your client machine run the following command.

C:\\> tftp 192.168.20.126 get test.txt Transfer successful: 11 bytes in 1 second, 11 bytes/s

If your TFTP is working you will get the above output.

### Install vsftp

If you want to enable FTP provosioning for Polycom phones  install vsftpd

#yum install vsftpd

Add Polycom phone user

#useradd PlcmSplp -d /tftpboot #passwd PlcmSplp Enter PlcmSplp as password

Then add PlcmSplp to FTP configuration

#edit /etc/vsftpd/vsftpd.conf Open the vsftpd.conf file and search for chroot\_list\_enable=YES Uncomment the line and make sure it is YES. Do the same for the following variables chroot\_list\_file=/etc/vsftpd/chroot\_list

Create vsftpd.chroot\_list in /etc/vsftpd/ and add the user PlcmSplp.

Save and close the file.

Configure DHCP Server

Configure DHCP sever to send boot sevrver ip along with the DHCP lease. Here I am using windows 2003 server. Find the screen shots

![](/images/1.png) ![](/images/set_pre.png)

![](/images/set_pre2.png)

![](/images/set_pre3.png)

![](/images/set_pre4.png)

![](/images/set_pre5.png)

Download and extract the lastest polycom firmware to /tftpboot directory. Download the provisioning module from [here](https://www.epillars.com) and install it into FreePBX from Module Admin Download the sample csv file from [here](https://www.epillars.com). Add the required extension,fullname,password and macaddress of the phones. Upload the csv file from the Polycom Provsioning Menu of the FreePBX. It will create all polycom configuration files required to register phones. Connect the phones. The phones will be upgraded with the new firmware. Every phones will be provisioned with PBX wide directory(You will get all PBX users extension numbers in directory)

If the phone is not downlading the firmware check the the boot server IP address in the phone. The boot server IP address should be the IP address of the PBX.

```
TFTP Configuration - Write Enabled

/etc/xinetd.d/tftp
service tftp
{
 socket_type = dgram
 protocol = udp
 wait = yes
 user = root
 server = /usr/sbin/in.tftpd
 server_args = -c -s -vv /tftpboot
 disable = no
 per_source = 11
 cps = 100 2
 flags = IPv4
}
```
