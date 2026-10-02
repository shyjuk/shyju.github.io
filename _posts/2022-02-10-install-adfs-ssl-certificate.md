---
title: "Install ADFS SSL Certificate"
date: 2022-02-10
categories: 
  - "technical"
tags: 
  - "adfs"
  - "adfs-certificate"
  - "installl-certificate"
---

**This site has a new home; please follow this link:**  
**https://www.shyju.in/posts/Install-SSL-Certificate-for-ADFS-server/**

1. Install the SSL certificate in server and get the certificate Thumbprint.

3. Run the below PowerShell command (_change the thumbprint with yours_)install it in ADFS. Use thumbprint with out spaces.  
    Set-AdfsSslCertificate -Thumbprint 'e5415105c8db76a659ea5ed23ac7d6fc8e9ebda8'
