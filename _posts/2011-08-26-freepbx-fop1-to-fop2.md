---
title: "FreePBX FOP1 to FOP2"
date: 2011-08-26
categories: 
  - "asterisk"
  - "freepbx"
  - "mysql"
  - "technical"
  - "voip"
tags: 
  - "asterisk-dubai"
  - "asterisk-flash-operator-panel"
  - "asterisk-uae"
  - "elastix"
  - "elastix-fop1-to-fop2"
  - "elastixfop2"
  - "flash-operator-panel"
  - "fop1-to-fop2"
  - "fop2"
  - "fop2-installation"
  - "ip-phones-dubai"
  - "voip-dubai"
  - "voip-uae"
---

### How to change FOP1 to FOP2 on Elastix

1\.  Download and install FOP2

```
cd /usr/src
wget http://www.fop2.com/file.php?file=20
tar -zxvf fop2-2.22-centos5-i386.tgz 
cd fop2
make && make install
```

2\. Add fop2 user to Asterisk Manager.

Edit /etc/asterisk/manager.conf

```
[fop2]
secret = psfop2
deny=0.0.0.0/0.0.0.0
permit=127.0.0.1/255.255.255.0
read = system,call,log,verbose,command,agent,user,config,command,dtmf,reporting,cdr,dialplan,originate
write = system,call,log,verbose,command,agent,user,config,command,dtmf,reporting,cdr,dialplan,originate
```

3\. Edit /usr/localfop2/fop2.cfg and change the following lines.

```
manager_user=fop2
manager_secret=psfop2
```

4.Edit /etc/amportal.conf and edit the uncomment the line starting FOPRUN and change it as following.

```
FOPRUN=false
```

5\. Edit /var/www/html/panel/op\_server.cfg and change listen\_port.

```
listen_port=4444
```

6\. Edit /etc/httpd/conf/httpd.conf and add the following line under  Aliases:

```
alias /panel /var/www/html/fop2
```

7\. Add FreePBX index file and change the fop2 ownership asterisk.

```
cp /var/www/html/fop2/index.html /var/www/html/fop2/index_amp.php
chown -R asterisk.asterisk /var/www/html/fop2
```

8\. Restart httpd,amportal and fop2 services.

```
/etc/init.d/httpd restart
chkconfig fop2 on
service fop2 start
/usr/sbin/amportal restart
```
