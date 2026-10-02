---
title: "VirtualBOX Boot to USB Drive"
date: 2016-05-11
categories: 
  - "technical"
  - "virtualbox"
  - "virtualization"
  - "vm"
---

Run the below command to make

```
VBoxManage internalcommands createrawvmdk -filename d:\usb.vmdk -rawdisk \\.\PhysicalDrive2
```

In this USB drive is PhysicalDrive2, which is shown in below screenshot as Disk 2

[![Disk\_Mgmt](/images/disk_mgmt.png)](/images/disk_mgmt.png)
