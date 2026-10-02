---
title: "Certificate for Apache with Open SSL"
date: 2009-08-19
categories: 
  - "technical"
tags: 
  - "apache"
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "security"
  - "voip-dubai"
  - "web-server"
---

1.Certificate Creation

- **_Generate Private Key_**

```bash
openssl genrsa -des3 -out shyju-pc.key 1024
```

if you don't want to put password(Apache will always ask for password when starting the service)  don't put -des3

- _**Generate CSR**_

```bash
openssl req -new -key shyju-pc.key -config "C:\\Apache\\conf\\openssl.cnf" -out shyju-pc.csr
```

- **Generate Self Signed certificate for time being**

```bash
openssl x509 -req -days 30 -in shyju-pc.csr -signkey shyju-pc.key -out shyju-pc.crt
```

- _**Export to ISS**_

```bash
openssl pkcs12 -export -out DigiCertBackup.pfx -inkey shyju-pc.key -in shyju-pc.crt -certfile "D:\\NetworkSolutions_CA.crt"
```

If you want to export the certificate to another Apache server, copy the SSL certificate, private key, and any intermediate certificates to the second server and configure `httpd.conf`.

**_Network Solutions gives [some instructions](https://customersupport.networksolutions.com/article.php?id=891) on their website that are outdated so it left me guessing on the correct order to create the SSLCertificateChainFile. Here is the correct order:_**

```text
UTNAddTrustServer_CA.crt
AddTrustExternalCARoot.crt
NetworkSolutions_CA.crt
```

**_Just take the stuff out of each file and copy/paste into a new file. Do not remove the BEGIN and END lines. Then, place the file somewhere on the server and in the apache config enter the full path to it like this: SSLCertificateChainFile /etc/httpd/conf/certs/network\_solutions\_combined\_2008.crt_**

2\. Edit httpd.conf

```apache
Listen 443
<VirtualHost _default_:443>
ServerName shyju-pc
SSLEngine on
SSLCertificateFile "C:\\Apache\\conf\\shyju-pc.crt"
SSLCertificateKeyFile "C:\\Apache\\conf\\shyju-pc.key"

SSLCertificateChainFile "C:\\Apache\\conf\\combined.crt"
SetEnvIf User-Agent ".*MSIE.*" nokeepalive ssl-unclean-shutdown
CustomLog logs/ssl_request_log "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
</VirtualHost>
```
