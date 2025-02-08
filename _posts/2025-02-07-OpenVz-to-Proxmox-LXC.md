---
title: OpenVz to Proxmox LXC
date: 2024-02-07
categories: [Linux, Tips, Proxmox]
tags: [linux,tips,commands,proxmox]     # TAG names should always be lowercase
---

Mount the OpenVz container on the OpenVZ host

{% raw %}
```
vzctl mount 101

```
{% endraw %}

Login to the Proxmox host and create a new LXC container with the same OS as the OpenVz container
Login to the LXC container and run the following rsync command 
{% raw %}
```
rsync -vaz --delete root@<OpenVZ Host>:/vz/root/101/ / --exclude '/proc/*' --exclude '/dev/*' --exclude '/sys/*' --exclude '/tmp/*' --exclude .git/

```
{% endraw %}

Login to the Proxmox host and run the following command to set IP address
{% raw %}
```
pct set 110 -net0 name=eth0,bridge=vmbr0,ip=192.168.90.35/24,gw=192.168.90.1

```
{% endraw %}
