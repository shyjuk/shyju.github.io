---
title: "Log SCP/SFTP traffic - Ubuntu"
date: 2020-10-27
categories: 
  - "technical"
---

We can log the file access through scp/sftp using the below  sshd configuration.

Need to edit /etc/ssh/sshd\_config  and add "-l DEBUG" at the end of the line.

```
Subsystem sftp  /usr/lib/openssh/sftp-server -l DEBUG
```

/etc/init.d/ssh restart  

Once the service is restarted the logs will be available in /var/log/auth.log
