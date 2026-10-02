---
title: "Enable NAT : OpenVZ Container"
date: 2016-11-09
categories: 
  - "technical"
---

```bash
vzctl set 135 --netfilter full --save --setmode restart
```

 

\*135 : Container ID
