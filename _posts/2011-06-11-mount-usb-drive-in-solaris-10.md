---
title: "Mount USB drive in SOLARIS 10"
date: 2011-06-11
categories: 
  - "technical"
tags: 
  - "format-usb"
  - "linux-command-mount"
  - "linux-usb-access"
  - "mount"
  - "solrais"
  - "usb"
  - "usb-in-solaris"
  - "usb-mount-commands"
---

\# rmformat

Looking for devices...

1\. Logical Node: /dev/rdsk/c0t1d0p0

Physical Node: /pci@0,0/pci-ide@1f,1/ide@0/sd@1,0

Connected Device: HL-DT-ST DVDRAM GSA-H42N RL00

Device Type: DVD Reader/Writer

2\. Logical Node: /dev/rdsk/c4t0d0p0

Physical Node: /pci@0,0/pci8086,d605@1d,7/storage@3/disk@0,0

Connected Device: C2931006 Flash Disk 5.00

Device Type: Removable

#mount -F pcfs /dev/dsk/c4t0d0p0:c /mnt/usb

Not that the device used in mount command is "/dev/dsk/c4t0d0p0" and not "/dev/rdsk/c4t0d0p0"

c4t0d0p1:c" here the ":c" means Windows C Drive like access.
