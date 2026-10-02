---
title: "OpenVZ: ovz-panel: Change container storage location"
date: 2015-08-24
categories: 
  - "technical"
tags: 
  - "container-location-shift"
  - "containers"
  - "datastore-change-openvz"
  - "openvz"
  - "openvz-change-container-storage-location"
  - "openvz-web-panel"
  - "ovz-panel"
  - "storage-location-change-openvz"
---

Prepare a new location for the containers.  Create root,private directories under the new location.

Create a new server template from OpenVZ Web Panel

[![ovz-new-server-template](/images/ovz-new-server-template.png)](/images/ovz-new-server-template.png)

 

Login to OpenVZ hardware node and find the newly created template file from the location /etc/vz/conf

![vz-conf-sample](/images/vz-conf-sample.png)

 

Edit the file and add the new root & private location in the configuration file.

![alternate-vz-location](/images/alternate-vz-location.png)
