---
title: PostFix disable SMTP Connections From Outside
date: 2023-10-23
categories: [Linux, Mail Server]
tags: [mail,smtp,postfix,tips]     # TAG names should always be lowercase
---

_Use this if you don't want to allow SMTP connection from outside the server._

To enhance the security of your Postfix mail server, you can limit SMTP connections to the local host. Follow these steps:

- Locate the line that reads:
  {% raw %}
```
smtp inet n - - - - smtpd

```
{% endraw %}

- Modify this line to restrict SMTP connections to the local host by changing it to:
  {% raw %}
```
127.0.0.1:smtp inet n - - - - smtpd

```
{% endraw %}

By making this adjustment, your Postfix mail server will only accept SMTP connections from the local machine (127.0.0.1). 
This simple configuration change will stop spammers from sending spam emails using your server.
