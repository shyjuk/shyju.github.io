---
title: "OpenSSL - Create a Self Signed Document Signing certificate based on an external CSR"
date: 2021-01-20
categories: 
  - "technical"
tags: 
  - "csr"
  - "digital-signature"
  - "openssl"
  - "pdf-sign"
  - "siging-pdf-files"
---

1. Install OpenSSL  
    ```bash
    yum install openssl
    ```
2. Edit /etc/pki/tls/openssl.cnf and change the \[policy\_match\] and make all optional
3. Create CA  
    ```bash
    openssl req -new -x509 -days 365 -key /etc/pki/CA/private/cakey.pem -out /etc/pki/CA/certs/cacert.pem
    ```
    

Country Name (2 letter code) \[XX\]:AE  
State or Province Name (full name) \[\]:Dubai  
Locality Name (eg, city) \[Default City\]:DSO  
Organization Name (eg, company) \[Default Company Ltd\]:My Company  
Organizational Unit Name (eg, section) \[\]:IT  
Common Name (eg, your name or your server's hostname) \[\]:My Company CA  
Email Address \[\]:me@mycompany.com

  
4\. Create index & serial file. Remove all lines from this if already exists.  
```bash
touch /etc/pki/CA/index.txt
echo '1000' > /etc/pki/CA/serial
```

If you don't have a CSR run the commands below to create a certificate.  

```bash
openssl req -utf8 -nameopt oneline,utf8 -new -key username_key.pem -out username_req.pem
```

```bash
openssl x509 -days 365 -CA /etc/pki/CA/cacert.pem -CAkey /etc/pki/CA/private/cakey.pem -CAserial /etc/pki/CA/serial -in username_req.pem -req -out username.pem
```

```bash
openssl pkcs12 -export -in username.pem -inkey username_key.pem -out username.p12
```

You can use this file(username.p12) to test digitally signing a pdf using acrobat reader.

[![](/images/image.png)](/images/image.png)

  
If you have an external CSR and you want to supply a certificate using that CSR run the below command. Here the CSR file is "dsa\_csr\_ad.txt"

```bash
openssl ca -cert /etc/pki/CA/cacert.pem -keyfile /etc/pki/CA/private/cakey.pem -in dsa_csr_ad.txt -out shyju_cert_ad.pem
```

Self Signed certificates will always show at least one signature has problems. You have to manually trust it to show as valid.

[![](/images/image-1.png)](/images/image-1.png)

OS Used : CentOS 7
