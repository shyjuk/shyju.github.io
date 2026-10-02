---
title: "Apache - Restrict/Block direct IP access"
date: 2022-01-04
categories: 
  - "technical"
tags: 
  - "apache"
  - "apache-ip-block"
  - "block-ip-bases-access"
  - "deny-access-by-ip"
  - "httpd"
  - "httpd-block-ip-based-access"
  - "linux"
  - "request-to-ip"
  - "virtual-hosts"
  - "virtualhost"
---

Under /etc/httpd/conf.d (RHEL based OS) create a new .conf file .Change the port and add SSL certificates and keys. All the request to IP will get a 403 Forbidden error and requests to sub.example.com will get severed from the directory /var/www/api.

```
Listen 9443

#All request to IP will be handled in this section
<VirtualHost _default_:9443>
SSLEngine on
SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
SSLCertificateKeyFile /etc/ssl/certs/apache-selfsigned.key
DocumentRoot /var/www/def
Redirect 403 /
UseCanonicalName Off
UserDir disabled
</VirtualHost>

#Name based requests 
<VirtualHost *:9443>
    DocumentRoot /var/www/api
    ServerName sub.example.com
    ServerAdmin admin@example.com
    SSLEngine on
    SSLCertificateFile /etc/httpd/certs/cert.pem
    SSLCertificateKeyFile /etc/httpd/certs/key.pem

    <Directory /var/www/api>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>

   ErrorLog /var/log/httpd/sub.example.com-error.log
   CustomLog /var/log/httpd/sub.example.com-access.log combined
</VirtualHost>

```
