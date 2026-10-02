---
title: "Modify XML Root element with PHP SimpleXMLElement"
date: 2011-02-03
categories: 
  - "technical"
tags: 
  - "php"
  - "root-element"
  - "simplexmlelement"
  - "xml"
---

**How to change XML Root element with PHP SimpleXMLElement class**

$extra\_config = "602\_0004f215cb72.cfg"; $xml\_resource = new SimpleXMLElement('config.xml', 0, true); $xml\_resource\['CONFIG\_FILES'\] = $extra\_config  . ", phone1.cfg, sip.cfg"; $xml\_resource-> asXML("config2.xml");

config.xml

<!--more-->

```
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<APPLICATION APP_FILE_PATH="sip.ld" CONFIG_FILES="phone1.cfg, sip.cfg" MISC_FILES="" LOG_FILE_DIRECTORY="" OVERRIDES_DIRECTORY="" CONTACTS_DIRECTORY="" LICENSE_DIRECTORY="">
 <APPLICATION_SPIP300 APP_FILE_PATH_SPIP300="sip_212.ld" CONFIG_FILES_SPIP300="phone1_212.cfg, sip_212.cfg"/>
 <APPLICATION_SPIP500 APP_FILE_PATH_SPIP500="sip_212.ld" CONFIG_FILES_SPIP500="phone1_212.cfg, sip_212.cfg"/>
 <APPLICATION_SPIP301 APP_FILE_PATH_SPIP301="sip_313.ld" CONFIG_FILES_SPIP301="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SPIP501 APP_FILE_PATH_SPIP501="sip_313.ld" CONFIG_FILES_SPIP501="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SPIP600 APP_FILE_PATH_SPIP600="sip_313.ld" CONFIG_FILES_SPIP600="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SPIP601 APP_FILE_PATH_SPIP601="sip_313.ld" CONFIG_FILES_SPIP601="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SSIP4000 APP_FILE_PATH_SSIP4000="sip_313.ld" CONFIG_FILES_SSIP4000="phone1_313.cfg, sip_313.cfg"/>
</APPLICATION>
```

config2.xml

```
<APPLICATION APP_FILE_PATH="sip.ld" CONFIG_FILES="602_0004f215cb72.cfg, phone1.cfg, sip.cfg" MISC_FILES="" LOG_FILE_DIRECTORY="" OVERRIDES_DIRECTORY="" CONTACTS_DIRECTORY="" LICENSE_DIRECTORY="">
 <APPLICATION_SPIP300 APP_FILE_PATH_SPIP300="sip_212.ld" CONFIG_FILES_SPIP300="phone1_212.cfg, sip_212.cfg"/>
 <APPLICATION_SPIP500 APP_FILE_PATH_SPIP500="sip_212.ld" CONFIG_FILES_SPIP500="phone1_212.cfg, sip_212.cfg"/>
 <APPLICATION_SPIP301 APP_FILE_PATH_SPIP301="sip_313.ld" CONFIG_FILES_SPIP301="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SPIP501 APP_FILE_PATH_SPIP501="sip_313.ld" CONFIG_FILES_SPIP501="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SPIP600 APP_FILE_PATH_SPIP600="sip_313.ld" CONFIG_FILES_SPIP600="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SPIP601 APP_FILE_PATH_SPIP601="sip_313.ld" CONFIG_FILES_SPIP601="phone1_313.cfg, sip_313.cfg"/>
 <APPLICATION_SSIP4000 APP_FILE_PATH_SSIP4000="sip_313.ld" CONFIG_FILES_SSIP4000="phone1_313.cfg, sip_313.cfg"/>
</APPLICATION>
```
