---
title: "Configuring Asterisk"
date: 2007-11-15
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "voip-dubai"
---

# Configuring Asterisk

##### Document Actions

Here we go through the basic configuration of Asterisk. We will set up analog telephones, SIP telephones, and IAX connections.

## FXO and FXS Channels

The difference between an FXO channel and an FXS channel is simply which end of the connection provides the dial tone. An FXO port does not generate a dial tone; it accepts one. A common example is the dial tone provided by your phone company. An FXS port provides both the dial tone and ringing voltage to alert the station user of an inbound call. Both interfaces provide bidirectional communication (i.e.,communication that is transmitted and received in both directions simultaneously).

If your Asterisk server has a compatible FXO port,you can plug a telephone line from your telephone company (or “telco”) into this port. Asterisk can then use the telco line to place and receive telephone calls. By that same token,if your Asterisk server has a compatible FXS port,you may plug an analog telephone into your Asterisk server, so that Asterisk may call the phone and you may place calls.

### Configuring an FXO Channel

We’ll start by configuring an FXO channel. The zaptel.conf file located in /etc/ is used to configure your hardware. The following minimal configuration defines an FXO port with FXS signaling:

```ini
fxsks=2

loadzone=us

defaultzone=us
```

After configuring zaptel.conf,you can load the drivers for the card. modprobe is used to load modules for use by the Linux kernel. For example,to load the wctdm driver, you would run:

```sh
modprobe wctdm
```

You can verify that the hardware and ports were loaded and configured correctly with the use of the ztcfg program:

```sh
/sbin/ztcfg -vv
```

### Configuring an FXS Channel

We’ll now configuring an FXS channel. The zaptel.conf file located in /etc/ is used to configure your hardware. In the example below we are configuring channel 1 to use FXO signaling,with the kewlstart signaling protocol:

```ini
fxoks=1
```

After loading the drivers for your hardware,you can verify their state with the use of /sbin/ztcfg –vv:

## Configuring SIP

The sip.conf file starts with a [\[general\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refgeneral) section,which contains the channel settings and default options for all users and peers defined within sip.conf. You can override the default settings on a per-user/peer basis by configuring them within the user/peer definition.

Domain Name System Service records (DNS SRV records) are a way of setting up a logical,resolvable address where you can be reached. This allows calls to be forwarded to different locations without the need to change the logical address. By using SRV records,you gain many of the advantages of DNS,whereas disabling them breaks the SIP RFC and removes the ability to place SIP calls based on domain names. (Note that if multiple records are returned,Asterisk will use only the first.) DNS SRV record lookups are disabled by default in Asterisk,but it’s highly recommended that you turn them on. To enable them,set srvlookup=yes in the [\[general\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refgeneral) section of sip.conf.

Each connection is defined as a user,peer,or friend. A user type is used to authenticate incoming calls,a peer type is used for outgoing calls,and a friend type is used for both. The extension name is defined within square brackets (\[\]). In this case,we have defined the extension john as a friend.

A secret is a password used for authentication. Our secret is defined as welcome. We can monitor the latency between our Asterisk server and the phone with qualify=yes, thereby determining whether the remote device is reachable. qualify=yes can be used to monitor any end device,including other Asterisk servers. By default,Asterisk will consider an extension reachable if the latency is less than 2,000 ms (2 seconds). You can configure the time Asterisk should use when determining whether or not a peer is reachable by replacing yes with the number of milliseconds.

If an extension is behind a device performing Network Address Translation (NAT), such as a router or firewall,configure nat=yes to force Asterisk to ignore the contact information for the extension and use the address from which the packets are being received. Setting host=dynamic will require the extension to register so that Asterisk knows how to reach the phone. To limit an endpoint to a single IP address or fully qualified domain name (FQDN),replace dynamic with the IP address or domain name. Note that this limits only where you place calls to,as the user is allowed to place calls from anywhere (assuming she has authenticated successfully). If you set host=static, the end device is not required to register.

## Configuring IAX Connections

### Inbound

In iax.conf,sections are defined with a name enclosed in square brackets (\[\]). Every iax.conf file needs at least one main section: [\[general\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refgeneral). Within the [\[general\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refgeneral) section,you define the settings related to the use of the IAX protocol,such as default codecs and jitter buffering. You can override the default codecs you specify in the [\[general\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refgeneral) section by specifying them within the user or peer definitions.

Within the [\[general\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refgeneral) section,you’ll need to add a register statement. The purpose of the register statement is to tell the FWD IAX server where you are on the Internet (your IP address). When a call is placed to your FWD number,the FWD servers do a lookup in their database and forward the call to the IP address associated with the FWD number.samples).

In the [\[iaxfwd\]](http://projects.cs.luc.edu/comp390-490/srg/documentation/configure#refiaxfwd) section,define the user for incoming calls with type=user. Then define where the incoming call will be handled within the dialplan,with context=incoming. To specify that the authentication for the incoming call will be done with an RSA public/private key pair,use auth=rsa. The public key is defined with inkeys=freeworlddialup. The freeworlddialup public key comes standard with Asterisk.

### Outbound

While an IAX user receives inbound calls; an IAX peer is used to place outbound calls. This section will set up iax.conf and extensions.conf so that you can place calls.

A peer is defined with type=peer. Use host to configure the server through which you will place calls (iax2.fwdnet.net). Your FWD account number and password will be used for authentication to the FWD network and are defined respectively with username and secret.

You can use the qualify=yes statement to occasionally check that the remote server is responding. The response time (latency) can be viewed from the Asterisk console with iax2 show peers. By default,a peer is considered unreachable after 2000 ms (2 seconds). You can customize the time period by replacing yes with the number of milliseconds.

The available codecs and the order of preference can be defined on a per-peer basis. disallow=all is used to reset any codec settings set previously. You can then allow the codecs you support and set their preference (from top to bottom),using the syntax allow=codec.

Use the iax2 show registry command from the Asterisk CLI to verify that you’ve registered successfully.

Excerpts in this section have been taken from O'Reilly's **Asterisk:The Future of Telephony**.
