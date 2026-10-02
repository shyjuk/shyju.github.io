---
title: "Asterisk on OpenWrt"
date: 2007-11-15
categories: 
  - "technical"
---

### Asterisk on OpenWrt

By [Joe Barr](http://clifornoobies.com/) on February 17, 2006 (8:00:00 AM)

[Asterisk](http://www.asterisk.org/) is free software that lets you create a fully functional, easily customizable, private branch exchange (PBX). Businesses like Asterisk because they can save money by using it, and because it is open source, they can add functionality to it easily and inexpensively. Asterisk is also becoming popular with home office users -- so much so that it spawned a new project called [Asterisk@Home](http://asteriskathome.sourceforge.net/), which released its 1.0 version last year. Now there's even a version of Asterisk that runs on OpenWrt, a Linux distribution designed to run on your wireless router (see "[OpenWrt nears prime time](http://www.linux.com/article.pl?sid=06/01/24/1733243)"). I found it to be worthwhile, but I wouldn't depend on it for my home office.

I installed Asterisk on OpenWrt White Russian RC4 on a Linksys WRT54GS wireless router. It's my first Asterisk installation. I admit that I scraped the knuckles on both hands getting Asterisk correctly configured, but now that I've done it, I would say it was worth all the frustrations it caused me. Not only do I now have a functional personal PBX, I've also learned a little about the black art of telephony along the way.

Installation was a snap. All I had to do was point my browser at the WRT54GS's IP address, log in at the OpenWRT Admin Console, and then click the install button next to the Asterisk and Asterisk-sounds packages. The install was finished, but I still had a long way to go.

But first, a word or two about what I had already done prior to installing Asterisk. I first had to find a telephone I could use with Asterisk. I chose a Budge Tone Grandstream BT-101, based on its relatively low cost. You have to configure the phone's user name, password, network parameters, and method of sending [dual-tone multi-frequency](http://en.wikipedia.org/wiki/DTMF) (DTMF) to get it to work with your Asterisk installation.

Next, I had to find a suitable VoIP provider. I found 11 zillion Internet phone companies, but not all of them are Asterisk-friendly, nor do they all allow customers to BYOD (bring your own device). When I found one -- [NuFone](http://www.nufone.net/) -- that did both, and seemed reasonably priced, I signed up for an account.

**Configure this way**

You must walk many paths, grasshopper, to Asterisk on OpenWRT configuration enlightenment. Sometimes those paths cross, double back, or wander deep into the land of Notworkski, but you must hack them until they work. I'll provide key portions of my configuration files in this story, but while they may work for me and my particular hardware and provider environment, they may not work for you. Use them as a guide rather than simply copying them into your /etc/asterisk directory.

| Resources |
| --- |
| The best documentation I've come across on Asterisk is a book from O'Reilly called _[Asterisk: The Future of Telephony](http://www.oreilly.com/catalog/asterisk/)_ , by Jared Smith, Jim Van Meggelen, and Leif Madsen.  Because the book is published under the [Creative Commons](http://creativecommons.org/licenses/by-nc-nd/2.0/ca/) license, it is available from both regular retail channels and by [free download](http://www.asteriskdocs.org/modules/tinycontent/index.php?id=11) on the Internet.  In addition, [the Asterisk Documentation Project](http://www.asteriskdocs.org/modules/news/) provides a lot of valuable documentation.   |

As you do the configuration, it may help to think of Asterisk as a PBX, the old-fashioned kind where an operator would physically connect an incoming call to the correct extension by plugging the correct patch-cord into the right socket. The Asterisk software does basically the same thing: it is all about patching local extensions to incoming or outgoing calls.

In my simple home office environment, my local extension -- the SIP BT-101 Internet phone -- needs to be able to talk to an Inter-Asterisk eXchange (IAX) network, NuFone. When we're done, Asterisk will speak the IAX protocol to NuFone and SIP to the phone.

If you look in /etc/asterisk after the install you'll see about 20 configuration files. Don't despair; you probably need to worry about only three of them: iax.conf, sip.conf, and extensions.conf.

All Asterisk configuration files are plain text files. The ground rules for hacking them are simple. Each conf file -- or at least each of the three covered in this article -- is divided into sections. Section names, sometimes referred to as contexts, are given within brackets, like this: `[section-name]`. Anything on a line following a semicolon is a comment and is ignored by the software. Some configuration statements take the form `item=_whatever_` . Others look like `item => _something_` .

Let's start with the iax.conf file. The IAX protocol thinks of its relationships as being between peers, users, or friends. The relationship between my Asterisk installation and Nufone is defined twice: one as peer, once as user. The settings in iax.conf allow Asterisk to register and handle outgoing calls from and incoming calls to the NuFone.

In the `[general]` section of iax.conf, I added the following line to register my account at NuFone:

`register => _username:password_@nufone.net`

In addition, I added two new sections to the file, both named NuFone. The first defines a peer relationship for outbound calls. The next defines a user relationship for inbound calls. My iax.conf looks like this:

```
[general]bandwidth=highallow=alldisallow=g723.1jitterbuffer=noforcejitterbuffer=noregister => user-name:password@providerIPaddresstos=lowdelayautokill=yes[NuFone]type=peeruser=usernamehost=providerIPaddresssecret=passworddisallow=allallow=ulawallow=gsm[NuFone]type=usersecret=passwordcontext=inbound
```

Now let's take a look at sip.conf. Remember, Asterisk is talking IAX to NuFone, and SIP to our phone, the BT-101, so sip.conf is where we tell Asterisk about the phone. My sip.conf looks like this, and should be a good starting point:

```
[general]context=default   ; Default context for incoming callsbindport=5060   ; UDP Port to bind to (SIP standard port is 5060)bindaddr=0.0.0.0  ; IP address to bind to (0.0.0.0 binds to all)srvlookup=yes   ; Enable DNS SRV lookups on outbound calls[authenticate]auth=username:password@192.168.1.1[bt101]type=friendcontext=internal        ; Where to start in the dialplan when this phone callscallerid=name ; Full caller ID, to override the phones confighost=dynamicuser=usernamesecret=passwordnat=no    ; there is not NAT between phone and Asteriskcanreinvite=no   ; allow RTP voice traffic to bypass Asteriskdtmfmode=rfc2833  ; either RFC2833 or INFO for the BudgeTonecall-limit=1   ; permit only 1 outgoing call and 1 incoming call at a time
```

Asterisk looks to a configuration called a dialplan to see how you want a call to be handled. You define dialplans in extensions.conf, which tells Asterisk how to handle phone calls. Before you craft a dialplan in extensions.conf, I recommend you have a copy of the book _[Asterisk: The Future of Telephony](http://www.oreilly.com/catalog/asterisk/)_ available for guidance.

**_Next: The dialplan_**
