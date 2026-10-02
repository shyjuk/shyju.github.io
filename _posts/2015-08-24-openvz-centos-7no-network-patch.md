---
title: "OpenVZ: Centos 7:No network patch"
date: 2015-08-24
categories: 
  - "technical"
tags: 
  - "ifup-venet00"
  - "no-network-centos-7"
  - "openvz-centos-7"
  - "venet0"
  - "venet00"
---

Add the patch text to file /root/ifup-aliases.patch

```bash
cd /etc/sysconfig/network-scripts/
patch < /root/ifup-aliases.patch
```

```
--- ifup-aliases.orig 2015-04-01 08:46:08.179879018 +0200
 +++ ifup-aliases 2015-04-01 08:46:52.558427785 +0200
 @@ -261,7 +261,8 @@
 is_available ${parent_device} && \
 ( grep -qswi "up" /sys/class/net/${parent_device}/operstate || grep -qswi "1" /sys/class/net/${parent_device}/carrier ) ; then
 echo $"Determining if ip address ${IPADDR} is already in use for device ${parent_device}..."
 - if ! /sbin/arping -q -c 2 -w ${ARPING_WAIT:-3} -D -I ${parent_device} ${IPADDR} ; then
 + /sbin/arping -q -c 2 -w ${ARPING_WAIT:-3} -D -I ${parent_device} ${IPADDR}
 + if [ $? = 1 ]; then
 net_log $"Error, some other host already uses address ${IPADDR}."
 return 1
 fi
```
