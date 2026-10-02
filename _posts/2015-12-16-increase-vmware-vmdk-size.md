---
title: "Increase VMWare vmdk size"
date: 2015-12-16
categories: 
  - "technical"
tags: 
  - "increase-vmdk-size"
  - "vmdk-size"
  - "vmware-increase-hard-disk-size"
---

<!--more-->vmkfstools -X 50G mydisk.vmdk

Which will set the Virtual HDD size to 50 GB
