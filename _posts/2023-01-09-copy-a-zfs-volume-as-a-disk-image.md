---
title: "Copy a ZFS Volume as a disk image"
date: 2023-01-09
categories: 
  - "technical"
tags: 
  - "clone"
  - "copy-zfs-disk"
  - "proxmox"
  - "virtualization"
  - "zfs"
  - "zfs-volume"
---

dd if=/dev/zvol/rpool/data/base-201-disk-0 of=win2k19.raw bs=1M

/dev/zvol/rpool/data/base-201-disk-0 is the disk file

win2k19.raw is the RAW disk file which you need to convert according to your needs (vmdk, qcow2 etc)
