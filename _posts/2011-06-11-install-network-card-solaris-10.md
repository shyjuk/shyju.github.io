---
title: "Install Network Card Solaris 10"
date: 2011-06-11
categories: 
  - "technical"
tags: 
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "linux"
  - "linux-commands"
  - "nic-with-solaris"
  - "solaris"
  - "solaris-networking"
  - "unix"
  - "unzip"
  - "voip-dubai"
---

First run ifocnfig -a to see all the network card attached to system which has ip address.

My NIC  is  " Intel Corporation PRO/100 VE Network Connection" installed in  Intel Pentum D 64 bit PC.

1\. Use the below command to see the connected NICs.

\# /usr/X11/bin/scanpci

#prtconf -pv | less

2.Download the appropriate driver file(tar file) from the other system and copy it to the solaris machine.

If you are using USB, see the previous mount article on [how to  mount  USB on solaris.](https://shyju.wordpress.com/2011/06/page/2/)

3.unzip the file if it is .gz(gunzip -d) and untar it (tar -xvf) .

4.Run the commands to compile and install the driver.

% cd /.../ife-x.x.x

% rm Makefile

% ln -s Makefile.amd64\_gcc Makefile

% rm obj

% ln -s amd64  obj

% /usr/ccs/bin/make

% /usr/ccs/bin/make install

% modload obj/ife

% /usr/sbin/drvconfig -i iprb

% touch /reconfigure

See the /dev directory to see the newly installed device.

\# ls /dev/ip\*

/dev/ip        /dev/ipf       /dev/ipnat     /dev/ipscan    /dev/ipstate

/dev/ip6       /dev/ipl       /dev/iprb      /dev/ipsecah   /dev/ipsync

/dev/ipauth    /dev/iplookup  /dev/iprb0     /dev/ipsecesp

Here /dev/iprb0  is my NIC.

Then reboot the machine.

Run the below command to get the DHCP address.

#ifconfig iprb0 auto-dhcp

\# ifconfig -a

lo0: flags=2001000849<UP,LOOPBACK,RUNNING,MULTICAST,IPv4,VIRTUAL> mtu 8232 index  1

inet 127.0.0.1 netmask ff000000

iprb0: flags=1000843<UP,BROADCAST,RUNNING,MULTICAST,IPv4> mtu 1500 index 2

inet 192.168.20.181 netmask ffffff00 broadcast 192.168.20.255

ether 0:19:d1:67:f7:cd

If you want to add static ip run the following command.

#sys-unconfig

Other usefull commands

update\_drv -a -i  'pci8086,1094.8086.d605' iprb

Files

/etc/defaultdomain       Set the default NIS domain name, if any, if it changed.

/etc/defaultrouter       Set the default router's IP address, if it changed.

/etc/hostname.le0        (or .hme0 or ?) Update if the hostname changed.

/etc/hostname6.le0       (or .hme0 or ?) Ditto, if you use IPv6.

/etc/hostname6.ip.tun0   Update if you use a IPv4/IPv6 tunnel (e.g., 6bone)

/etc/nodename            Update if the hostname changed.

/etc/nsswitch.conf       Update if your name resolution method/order changed.

/etc/resolv.conf         Update if your name servers/domain changed (DNS only).

/etc/inet/hosts          Make sure your IP address is updated or added here.

List your FQDN is first, before the short hostname.

E.g., "192.168.128.64 foo.bar.com foo"

/etc/inet/ipnodes        IPv6 version of hosts file (Solaris 8+).

/etc/inet/netmasks       Set your network number & netmask, if it changed.

/etc/inet/networks       Set your network name, if it changed.

/etc/net/ticlts/hosts    For the streams-level loopback interface.

/etc/net/ticots/hosts    For the streams-level loopback interface.

/etc/net/ticotsord/hosts For the streams-level loopback interface.
