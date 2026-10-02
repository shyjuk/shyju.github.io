---
title: "OpenSSL Apache-Win32"
date: 2010-08-26
categories: 
  - "technical"
tags: 
  - "apache"
  - "https"
  - "openssl"
  - "win32"
---

```cmd
G:\WAMP\Apache\bin>set OPENSSL_CONF=G:\WAMP\Apache\conf\openssl.cnf
```

Create a certificate signing request:

```cmd
G:\WAMP\Apache\bin>openssl req -new -out crm.epillars.local.csr
```

```text
Loading 'screen' into random state - done
Generating a 1024 bit RSA private key
..........................++++++
....++++++
writing new private key to 'privkey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank.
For some fields there will be a default value.
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:AE
State or Province Name (full name) [Some-State]:Dubai
Locality Name (eg, city) []:Dubai Silicon Oasis
Organization Name (eg, company) [Internet Widgits Pty Ltd]:ePillars Systems L.L.C
Organizational Unit Name (eg, section) []:IT
Common Name (eg, YOUR name) []:crm.epillars.local
Email Address []:shyju@epillars.local

Please enter the following 'extra' attributes to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Remove the passphrase from the private key:

```cmd
G:\WAMP\Apache\bin>openssl rsa -in privkey.pem -out crm.epillars.local.key
Enter pass phrase for privkey.pem:
writing RSA key
```

Sign the certificate request:

```cmd
G:\WAMP\Apache\bin>openssl x509 -in crm.epillars.local.csr -out crm.epillars.local.cert -req -signkey crm.epillars.local.key -days 3650
```

```text
Loading 'screen' into random state - done
Signature ok
subject=/C=AE/ST=Dubai/L=Dubai Silicon Oasis/O=ePillars Systems L.L.C/OU=IT/CN=crm.epillars.local/emailAddress=shyju@epillars.local
Getting Private key
```

Convert the certificate to DER format:

```cmd
G:\WAMP\Apache\bin>openssl x509 -in crm.epillars.local.cert -out crm.epillars.local.der.crt -outform DER
```

Copy the certificate and key into Apache's SSL configuration directory:

```cmd
G:\WAMP\Apache\bin>copy crm.epillars.local.cert ..\conf\ssl
G:\WAMP\Apache\bin>copy crm.epillars.local.key ..\conf\ssl
```

Add the following lines to `httpd.conf`:

```apache
Listen 443
SSLSessionCache "shmcb:D:/WAMP/Apache/logs/ssl_scache(512000)"
SSLSessionCacheTimeout 300

<VirtualHost _default_:443>
  DocumentRoot "D:/WAMP/web/sugarcrm-5.1.0b/htdocs"
  ServerName shyju-pc:443
  ServerAdmin admin@shyju-pc
  ErrorLog "D:/WAMP/Apache/logs/error.log"
  TransferLog "D:/WAMP/Apache/logs/access.log"
  SSLEngine On
  SSLCertificateFile conf/ssl/crm.epillars.local.cert
  SSLCertificateKeyFile conf/ssl/crm.epillars.local.key
</VirtualHost>
```
