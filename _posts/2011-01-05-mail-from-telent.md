---
title: "Mail from Telent"
date: 2011-01-05
categories: 
  - "technical"
tags: 
  - "base-64-encode"
  - "command-line-email"
  - "decode"
  - "email-encode"
  - "email-from-telnet"
  - "encode"
  - "mail-delivery-telnet"
  - "send-email-commands"
  - "send-email-using-windows-commands"
  - "telnet-and-email"
---

telnet mail.epillars.local 25

220 MAIL.epillars.local Microsoft ESMTP MAIL Service ready at Wed, 5 Jan 2011 11:48:11 +0400 helo 250 MAIL.epillars.local Hello \[192.168.20.114\] auth login 334 VXNlcm5hbWU6

c2tAZXBpbGxhcnMubG9jYWw= <[BASE64 encoded](http://ostermiller.org/calc/encode.html) Username 334 UGFzc3dvcmQ6 ZXBpbGxhcnM=<[BASE64 encoded](http://ostermiller.org/calc/encode.html) Password 235 2.7.0 Authentication successful mail from:sk@epillars.local 250 2.1.0 Sender OK rcpt to:administrator@epillars.local 250 2.1.5 Recipient OK data 354 Start mail input; end with <CRLF>.<CRLF> subject:test message

Hello, Testing .

250 2.6.0 <7189d48e-5f1a-4962-a2eb-e52cfae5ee28@MAIL.epillars.local> Queued mail for delivery 500 5.3.3 Unrecognized command quit 221 2.0.0 Service closing transmission channel

http://www.youtube.com/watch?v=wOxj\_HvnLmE
