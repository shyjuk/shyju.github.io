---
title: "Asterisk on OpenWrt(Dial Plan)"
date: 2007-11-15
categories: 
  - "technical"
tags: 
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "voip-dubai"
---

### Asterisk on OpenWrt

By [Joe Barr](http://clifornoobies.com/) on February 17, 2006 (8:00:00 AM)

  
[Asterisk](http://www.asterisk.org/) is free software that lets you create a fully functional, easily customizable, private branch exchange (PBX). Businesses like Asterisk because they can save money by using it, and because it is open source, they can add functionality to it easily and inexpensively. Asterisk is also becoming popular with home office users -- so much so that it spawned a new project called [Asterisk@Home](http://asteriskathome.sourceforge.net/), which released its 1.0 version last year. Now there's even a version of Asterisk that runs on OpenWrt, a Linux distribution designed to run on your wireless router (see "[OpenWrt nears prime time](http://www.linux.com/article.pl?sid=06/01/24/1733243)"). I found it to be worthwhile, but I wouldn't depend on it for my home office.

Creating a dialplan is a little bit like programming. You may remember that in iax.conf and sip.conf, we made entries identifying the context of whatever device or channel was being defined. That entry tells Asterisk where to look in extensions.conf for guidance on the connection being requested.

Here is my simple extensions.conf.

```
[general]
static=yes
writeprotect=no
autofallthrough=yes
clearglobalvars=no
priorityjumping=no
[globals]
TRUNKMSD=1     ; MSD digits to strip (usually 1 or 0)
OUTBOUNDTRUNK=IAX2/warthawg@NuFone
NUFONENUMBER=8005551212    ; phony number
[internal]
include => inbound
include => outbound
exten => 101,1,Dial(SIP/bt101,15)
exten => _9NXXNXXXXXX,1,Goto(outbound|BYEXTENSION|1)
[inbound]
exten => 8005551212,1,Answer
exten => 8005551212,2,Background(enter-ext-of-person)
exten => 101,1,Dial(SIP/bt101,15)
exten => 101,102,Playback(ext-is-busy)
exten => 101,103,Hangup
exten => i,1,Playback(invalid)
exten => i,2,Goto(inbound,8004288951,2)
exten => t,1,Hangup\
[outbound]
exten => _91NXXNXXXXXX,1,Dial(${OUTBOUNDTRUNK}/${EXTEN:1})
exten => _91NXXNXXXXXX,2,Congestion
exten => _91NXXNXXXXXX,102,Congestion
[default]
exten => s,1,Dial(SIP/bt101,15)
```

Let's go over the \[inbound\] context shown above, which contains the rules for Asterisk to apply to incoming calls from my DID number from NuFone.

| Asterisk-speak: A basic primer |
| --- |
|   **Dialplan** - the instructions that control the execution of your PBX: making connections for incoming, outgoing, and internal calls.  **DID** - Direct Inward Dialing. A method of overloading physical phone lines so that you can have a greater number of extensions than actual lines. In a small business, for example, there may only be four phone lines, but -- thanks to the magic of DID -- six or eight extensions can share them, each reachable by its own distinct phone number. The phone company passes the number dialed to the PBX, and the PBX patches the call to the correct extension. So even though you have only four lines, you can pretend to have twice that many. The only caveat is that you are always limited by the fact that you can't have more concurrent calls than you have real lines. If you've ever tried to make a call from an extension in a business but have gotten a message saying all lines are busy, DID is the culprit.  **IAX** - Inter-Asterisk eXchange protocol, Asterisk's protocol for data transport. These days, IAX actually refers to IAX2, the second generation of IAX. IAX can handle a number of different codecs and types of data, from voice to video.  **Jitter** - An abrupt stop/start/change in the rate of incoming traffic which causes the audio signal to break up and perhaps to garble words. Similar to pixelation on a video stream, except it's in your ear.  **PBX** - Private Branch eXchange. The equipment that routes incoming and outgoing calls at a location to the phones on the desks at that location. Basically, a switchboard. Asterisk is an open source software PBX.  **POTS** - Plain Old Telephone Service. What you get from Ma Bell or one of her children or competitors.  **PSTN** - Public Switched Telephone Network.  **RTP** - Real-time Transport Protocol. An Internet protocol (per [RFC 1889](http://www.faqs.org/rfcs/rfc1889.html)) for the transport of audio and video.  **SIP** - Session Initiation Protocol. SIP is an IETF standard for managing interactive sessions requiring multimedia, and is widely used for VOIP. SIP does not pass data; it only manages the sessions. RTP is used to pass the data after SIP sets up the session.   |

When a call is received on the inbound IAX connection defined in iax.conf, it is identified by the number the caller dialed when calling me. Any line that begins `exten =>` applies only to the extension or group of extensions following the `=>`. I say extension or group of extensions because it can either be a discrete phone number, like 8005551212, or a pattern for an extension that may include a number of different extensions. The general format for the `exten =>` directive is extension, priority, command. The priority determines the order of execution, not the order in which they appear in the file.

The first three lines of the \[inbound\] context are only executed when the extension is that of the DID number provided by NuFone. Regardless of the order those three lines appear in, they are executed by priority, with the lowest number being the highest priority.

Therefore, the Answer command is executed first, the Background second. The Background command plays a recording asking the caller to enter the extension he desires.

You could use Playback on this line instead of Background, but if you use Background, Asterisk will listen for the extension being entered while the message is being played. If you use Playback, Asterisk won't start listening for the extension until the message has been played, which could be annoying for callers who already know what the message says.

Note that the extension changes in the third line. If and only if the caller enters extension 101 does Asterisk dial the BT101 extension. If the caller dials 101 and someone answers, the connection is complete and we are done.

If the extension dialed is busy, Asterisk bumps the priority by adding 101 to the priority that dialed the call, so that any following commands with a priority of less than 102 are not executed. In my dialplan, a busy extension plays a message saying I'm busy and asks the caller to call back later, then hangs up.

Asterisk provides two special codes for the extension for easy handling of both invalid and busy extensions. Notice the first two lines following the Hangup command. Both use the special `i` code, meaning an invalid -- unhandled -- extension was entered. Those two lines play a message to the caller, then route the call back to the Background command, asking the caller to enter the extension desired.

The last command in the \[incoming\] context uses the special `t` extension code, which catches calls where no extension is entered within 10 seconds. It simply hangs up the phone.

**Starting Asterisk for the first time**

To start Asterisk running on OpenWrt, enter `asterisk -cvvv` at the command line. This will open an Asterisk console for you, where you can monitor and control its operation.

While you are tweaking your configuration, it may be wise to enter the following two commands when you first start Asterisk.

```
set verbose 10set debug 10
```

That will make Asterisk as verbose as it is possible for it to be, and when you're shaking down your configuration, especially the dialplan, that's a good thing. For other commands available at the Asterisk console, enter `help`.

When things don't go as you think they should, turn to the book first, but remember that the version of Asterisk ported to OpenWrt is 1.07, and the current Asterisk version is 1.2.x, so the documentation may not match reality on OpenWrt. The #asterisk and #openwrt IRC channels on Freenode.net are also handy resources.

Having your own PBX is fun. [Click Here](http://www.linux.com/var/slashimages/8a7287b1b9dc032de33f1c43f0358cb4.octet-stream) for a sample of one my unavailable messages callers might be greeted with.

**Conclusions about Asterisk on OpenWrt**

I have experienced some jitter with Asterisk on OpenWrt, but most of the time the voice quality is fine. An active telephone call will have some impact on your Internet surfing and file downloading, but assuming you have broadband access, it won't be significant, and vice versa.

I've since used bwm-ng -- a bandwidth monitor -- and measured Asterisk usage at 20KB a second each way during a SIP call. You probably will not notice it at all on your broadband connection, even though a big download or upload might take a little longer.

Memory and storage are the two big constraints on Asterisk on OpenWrt. A voice mail module is available, but with such scarce resources for storage, I opted not to use it. Running a second or third line would also be expensive in terms of memory, and might be more than OpenWrt could handle.

In my opinion, Asterisk on OpenWrt is a fun experiment, a good way to learn about telephony, and an easy way to learn Asterisk. But I wouldn't want my small business, or even my home office, to depend on it for voice communications.

[<<](http://www.linux.com/articles/52030?page=1 "First page") [prev (1/2)](http://www.linux.com/articles/52030?page=1 "Previous page")
