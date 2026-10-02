---
title: "Asterisk - How to detect a busy tone"
date: 2011-09-12
categories: 
  - "asterisk"
  - "freepbx"
  - "mysql"
  - "technical"
  - "voip"
tags: 
  - "3cx"
  - "asterisk-call-disconnect-tone-settings"
  - "asterisk-call-hang-up"
  - "asterisk-call-progress-tones"
  - "asterisk-dubai"
  - "asterisk-tone-frequency"
  - "asterisk-uae"
  - "busy-disconnect"
  - "call-disconnect"
  - "dahdi-call-disconnect-tone-settings"
  - "detect-busy"
  - "find-busy-tone-frequency"
  - "ip-phones-dubai"
  - "problem-with-call-hang-up"
  - "switchvox"
  - "voip-dubai"
  - "voip-uae"
---

### **Asterisk without "Disconnect Supervision"**

It is very hard to configure an asterisk system if your telco is not providing  [Disconnect supervision](http://www.voip-info.org/wiki/view/Asterisk+Disconnect+Supervision) on your PSTN/Analog line. Even the developers of digium "Switchvox" could not solve the "great call hangup issue" on their great PBX  till now (12th Sep. 2011), because many telecom providers all over the world do not support  this method of call disconnection.

So what happens if you are using asterisk and do not have disconnect supervision on your analog line. If you have not properly configured asterisk with busydetect=yes/callprogress=yes in dahdi/zapata configuration you  will end up  "always busy analog lines"  and you may have to restart the PBX  to receive a call/make call. As an example , I'm using asterisk and if somebody calling me,the call rings my phone via asterisk and  I'm not at my desk to attend the call, so the callee hangs up his phone. But the asterisk will keep ringing my phone because it will not detect the "call disconnect tone"  which is send by the telco when the callee hangup the call. So that PSTN/Analog line remains busy until you manually pick up  phone and disconnect or restart asterisk. This may happen always when caller/callee fails to hangup the call., ie call hang up from one end will not release the line for another call.

Try the below settings before you start.  Try increasing rxgain if pbx could not detect the busytone successfully.

Add busypattern if required. If your TSP supports polarity reversal(disconnect supervision) hanguponpolarityswitch is the best option.

Edit the file /etc/asterisk/chan\_dahdi.conf

rxgain=5.0

busydetect=yes

busycount=3

```
;busypattern=500,500
```

## **The Solution**

##     One easy solution is get the appropriate busy tone/disconnect tone settings from your telco(you will never get it ;) ) or from [World PSTN Tone Database](http://www.3amsystems.com/wireline/tone-search.htm). If your telco tones are different than those they have given, then you may have to record the busy/call disconnect tone(Use [3CX Phone](http://www.3cx.com/VOIP/voip-phone.html), or [DAHDIBarge](http://www.voip-info.org/wiki/view/Asterisk+cmd+ZapBarge)). Then use [Audacity](http://audacity.sourceforge.net/download/windows)/[Wavesurfer](http://www.speech.kth.se/wavesurfer/index.html) to find the on/off cadence timing on the recorded call. Find the screenshots. **Audacity** [![](/images/disconnect-tone.png "disconnect-tone")](/images/disconnect-tone.png) 

Here the ON timing is around 325 millisecond and the OFF timing is also the same.

So you got the busy tone/disconnect tone pattern  325/325. For some telco there may be multiple on/off settings ,like 325 on 325 off then 400 on and 350 off, then it repeats.ie 325/325/ 400/350.

**Wavesurfer [![](/images/open.png "Open")](/images/open.png)** 

Open the recorded file in Wavesurfer and select choose Configuration as Waveform.

![](/images/zoom_in.png "Zoom_In")

Zoom in the waveform.

[![](/images/spectrum_controls.png "Spectrum_Controls")](/images/spectrum_controls.png)

Right click in the  Spectrogram Pane to get the Spectrogram Controls.

[![](/images/frequency.png "Frequency")](/images/frequency.png)

Put the analysis window length to maximum to get the narrow spectrum line. Mouse over the spectrum line and see the status bar to get the frequency.

[![](/images/length1.png "Length1")](/images/length1.png)

Select the wave form from start to end so that you will get the ON timing as a tool tip.

[![](/images/length2.png "Length2")](/images/length2.png)

Select the blank area to get the OFF timing.

**PIKA - The easy PBX**

If you are using pika PBX you can straight away use this settings to configure the PBX. Edit the file /etc/pika/inccpa.cfg. Add new pattern under \[callpa\_settings\]. Create the pattern context with the new tone settings.

If you got the tone on/off pattern as 400 on,350 off, 230 on,530 of as "Call disconnect" Tone. then the configuration will be as follows.

There are more patterns.showed only newly added pattern.

```
[callpa_settings]
pattern4=cp_huntbusy
[cp_huntbusy]
type=4
tolerance=30
cadences=2
states=4
ignorestates=1
state0=400
state1=350
state2=230
state3=530
```

**DAHDI Configuration for Normal PC/Server based Asterisk installation**

The main configuration files are /etc/asterisk/chan\_dahdi.conf &  /etc/asterisk/dahdi-channels.conf

.. to be continued..

Worth reading: [http://www.fredshack.com/docs/asterisk.html](http://www.fredshack.com/docs/asterisk.html)
