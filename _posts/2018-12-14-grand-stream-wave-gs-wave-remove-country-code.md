---
title: "Grand stream Wave (GS Wave) Remove country code"
date: 2018-12-14
categories: 
  - "technical"
---

Goto Account Settings > Account>DialPlan > enable DialPlan

Under DialPlan Settings add following.

This will remove leading +91 from any dialed digits

```
{<+91=>x+ | x+ | +x+ | *x+ | *xx*x+ } 
```
