---
title: "OpenVZ Failed to start SSH -Work around"
date: 2019-07-02
categories: 
  - "technical"
---

```bash
mkdir /var/run/sshd
/etc/init.d/ssh restart
```

https://serverfault.com/a/952903/522655
