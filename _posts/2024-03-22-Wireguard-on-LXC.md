---
title: VPN on LXC Container - LXC Commands
date: 2024-03-22
categories: [Linux, Tips]
tags: [linux,tips,commands]     # TAG names should always be lowercase
---

Before installing Wireguard on LXC, run these commands.

{% raw %}
```
lxc config set lxd-vpn raw.lxc "lxc.mount.entry = /dev/net dev/net none bind,create=dir"
lxc config set lxd-vpn linux.kernel_modules=ip_tables
lxc config set lxd-vpn raw.lxc "lxc.cgroup2.devices.allow = c 10:200 rwm"
lxc config device add lxd-vpn port proxy listen=udp:0.0.0.0:51820 connect=udp:0.0.0.0:51820

```
{% endraw %}

