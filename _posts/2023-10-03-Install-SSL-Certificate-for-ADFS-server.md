---
title: Secure Your ADFS Server - Installing an SSL Certificate
date: 2023-10-10
categories: [Windows, ADFS]
tags: [windows, adfs, ssl certificate]     # TAG names should always be lowercase
---

1. Install the SSL certificate in server and get the certificate Thumbprint.
 ![Certificate Thumbprint](https://github.com/shyjuk/shyjuk.github.io/assets/9428173/b959a8b1-7ffd-47f7-af91-e5300e83c57e)

3. Run the below powershell command (change the thumbprint with yours)install it in ADFS. Use thumbprint with out spaces.
{% raw %}
```
  Set-AdfsSslCertificate -Thumbprint 'dddfc3dd49e39d9e469cc2f37d091a92545e4ac4'
```
{% endraw %}
