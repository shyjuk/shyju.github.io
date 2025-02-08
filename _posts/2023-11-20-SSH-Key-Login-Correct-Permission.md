---
title: SSH Key based Login - Correct Permission
date: 2023-11-20
categories: [Linux, Tips]
tags: [linux,tips,commands]     # TAG names should always be lowercase
---

The home directory of a user should be writable only by that user. The commands to apply correct permission is below.

{% raw %}
```
chmod go-w /home/server-user
chmod 700 /home/server-user/.ssh
chmod 600 /home/server-user/.ssh/authorized_keys

```
{% endraw %}

