---
title: "Import Virtualbox VM to VMware vSphere"
date: 2014-12-17
categories: 
  - "technical"
tags: 
  - "convert-vmdk"
  - "virtualbox-to-vmware"
  - "virtualbox-to-vsphere"
  - "vmdk-upload"
  - "vmware-esxi"
---

1. Upload the Virtualbox vm's vmdk file to vmware datastore.
2. ssh to vmware vSphere/ ESXi and convert the uploaded disk. The snapshots will not work if the file is not converted. command : _vmkfstools -i <source>.vmdk <target>.vmdk -d thin_
3. Create a new VM in with vSphere client and attach the converted disk to it
