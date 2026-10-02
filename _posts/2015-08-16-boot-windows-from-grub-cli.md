---
title: "Boot Windows from grub CLI"
date: 2015-08-16
categories: 
  - "technical"
tags: 
  - "boot-to-windows-from-grub"
  - "dual-boot"
  - "grub"
  - "grub-to-windows"
---

At the linux boot menu press e to goto edit mode & ctrl+c to to grub CLI

Run following commands to boot into windows

```
insmod chaininsmod ntfsset root=(hd0,msdos1)chainloader +1boot
```

### **/dev/sda does not have any corresponding BIOS drive**

```
grub-install  --recheck /dev/sda
```
