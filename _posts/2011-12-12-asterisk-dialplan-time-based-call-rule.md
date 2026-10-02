---
title: "Asterisk Dialplan - Time Based Call Rule"
date: 2011-12-12
categories: 
  - "technical"
tags: 
  - "asterisk-office-timing-settings"
  - "asterisk-time-based-rules"
  - "asterisk-timed-calls"
  - "block-unauthorized-calls"
  - "call-timing-restriction-asterisk"
  - "time-based-call-rule"
  - "time-based-dial-plan-asterisk"
---

If you want to restrict outgoing calls in asterisk for a certain time in day , use the below dialplan.

If you are using freepbx edit  "/etc/asterisk/extensions\_custom.conf" and add a new context., and assign that  context to the users.

```ini
[time-rules]
exten => _XXXX.,1,GotoIfTime(8:30-18:45|sun-thu|*|*?from-internal,${EXTEN},1)
exten => _XXXX.,n,Playback(prepaid-auth-fail)
```

[![](/images/dialplan.png "DialPlan")](/images/dialplan.png)

This rule will deny users from making calls(if the number of digits higher than 4 digiits) from 6:45 PM to 8:30 AM Sunday through Thursday and whole Friday.

For more info on command GotoIfTime() visit [VoIP Info](https://www.voip-info.org/wiki/view/Asterisk+cmd+GotoIfTime).
