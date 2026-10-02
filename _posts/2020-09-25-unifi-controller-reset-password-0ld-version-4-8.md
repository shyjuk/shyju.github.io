---
title: "UniFi Controller : Reset password (for old version 4.8)"
date: 2020-09-25
categories: 
  - "technical"
tags: 
  - "bash-script"
  - "hash-password"
  - "mkpasswd"
  - "mongodb"
  - "mongodb-with-bash"
  - "password-reset"
  - "sha-512-hash"
  - "unifi"
  - "unifi-password-reset"
---

SSH into UniFi controller server

Create a bash script by copying the below code to a file (eg: ufpreset.sh)  

```
#!/bin/sh
username=$(mongo --port 27117 ace --eval 'db.admin.findOne().name' | tail -1)
chash=$(mongo --port 27117 ace --eval 'db.admin.findOne().x_shadow' | tail -1)
echo "\033[1;34mUniFi Controller \033[0m"
echo "\033[1;34mCurrent Admin User:\033[0m" $username
echo "\033[1;34mCurrent Password Hash:\033[0m" $chash
echo "\033[1;34mPlease enter a new password for UniFi Controller:\033[0m"
read unipass
[ -z "${unipass}" ]
str=mkpasswd -m sha-512 $unipass
mongo --port 27117 ace --eval 'db.admin.update( { "name" : '\"${username}\"' }, { $set : { "x_shadow" : '\"${str}\"' } } )'
echo "\033[1;35mPassword reset to ${unipass}\033[0m"
newhash=$(mongo --port 27117 ace --eval 'db.admin.findOne().x_shadow' | tail -1)
echo "\033[1;34mNew Password Hash:\033[0m" $newhash
```

Make it executable  
`chmod +x ufpreset.sh`

Run the script to set new password  
`./ufpreset.sh`

![](/images/unifi_password_reset.png)
