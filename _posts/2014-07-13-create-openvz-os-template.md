---
title: "Create OpenVZ OS Template"
date: 2014-07-13
categories: 
  - "technical"
tags: 
  - "create-os-template"
  - "openvz"
  - "openvz-template"
  - "template"
---

1. Login to OpenVZ(Hardware Node).
2. Start the VM for the template creation.(replace 105 with your container ID)
    
    ```
    vzctl start 105
    ```
    
3. Remove the IP from the VM.
    
    ```
    sudo vzctl set 105 --ipdel all --save
    ```
    
4. Stop the container.
    
    ```
    vzctl stop 105
    ```
    
5. Create template
    
    ```
    cd /vz/private/105/
    ```
    
    ```
    tar -czf /vz/template/cache/<OS>-<ARCH>.tar.gz ./
    ```
    
6. Create the configuration file.
    
    ```
    cp /etc/vz/dists/centos.conf /etc/vz/dists/template-name.conf
    ```
