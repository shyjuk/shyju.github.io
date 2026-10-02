---
title: "Linksys SPA decrypt cfg"
date: 2017-02-14
categories: 
  - "technical"
tags: 
  - "config-decrypt"
  - "decrypt"
  - "linksys-spa"
  - "spa-cfg-decrypt"
---

Use the openssl command to decrypt

```bash
openssl enc -d -aes256 -k <key> -in <macaddress>.cfg -out rep.xml
```

<key> is the password which you have put in SPA interface.

[![linksys](/images/linksys.png)](/images/linksys.png)
