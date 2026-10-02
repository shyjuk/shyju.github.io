---
title: "Asterisk - Skill Based Routing"
date: 2011-07-09
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "agent"
  - "asteirsk"
  - "asterisk-dubai-uae"
  - "call-center"
  - "call-distrubution"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "queue"
  - "skill-based-routing"
  - "voip-dubai"
---

## \*\*skills or specialties = priority 1 / priority 2 / priority 3

agent 1 = mac

agent 2 = linux

agent 3 = linux

agent 4 = mac / linux

agent 5 = linux / mac

When agent 1 is busy, next call should go to agent 4. But when both agent 1 and 4 is busy (both have priority skill for MAC), call should go to agent 5 (can accept calls for MAC) because it has priority 2 for MAC. Is this possible?

Asterisk doesn't really have skills based routing yet, but you can do the same thing with multiple queues. Each queue being a skill. Below you will find my interpretation of your requirements. In your example, agent 4 and 5 are members of both queues, but have their penalties set appropriately.

queues.conf

```
[general]
autofill=yes 

[linux]
; Put whatever other queue options you want here
strategy=rrmemory
member => Agent/agent2,1
member => Agent/agent3,1
member => Agent/agent4,3
member => Agent/agent5,2 

[mac]
; Put whatever other queue options you want here
strategy=rrmemory
member => Agent/agent1,1
member => Agent/agent4,2
member => Agent/agent5,3
```

[https://forums.digium.com/viewtopic.php?t=19527](https://forums.digium.com/viewtopic.php?t=19527 "Skill Based Routing")
