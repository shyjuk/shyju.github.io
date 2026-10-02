---
title: "Asterisk GUI Installation"
date: 2010-06-23
categories: 
  - "asterisk"
  - "technical"
  - "voip"
---

![Asterisk Login Page](/images/ast.png) **Requirements**

Before you can install Asterisk GUI you need to install Asterisk and all the dependencies. Download and install them from [Asterisk website](https://downloads.asterisk.org/). **Getting Asterisk GUI**

Open a terminal and type: `cd /usr/src/ svn checkout https://svn.digium.com/svn/asterisk-gui/branches/2.0/ gui`

The Asterisk GUI source files will be downloaded to /usr/src/gui folder

**Instructions**

Use following commands to install it.

```bash
cd /usr/src/gui
./configure
make
make install
```

Edit /etc/asterisk/http.conf file and make following changes.

```ini
enabled = yes
bindaddr = 0.0.0.0
bindport = 80
enablestatic = yes
redirect = / /static/config/cfgbasic.html
```

Edit /etc/asterisk/manager.conf file and make following changes.

```ini
[general]
enabled = yes
webenabled = yes
port = 5038
bindaddr = 0.0.0.0

[admin]
secret = password
read = system,call,log,verbose,agent,user,config,dtmf,reporting,cdr,dialplan
write = system,call,agent,user,config,command,reporting,originate
```

Goto the same folder from where you installed Asterisk GUI,and check the configuration.

```bash
cd /usr/src/gui
make checkconfig
```

You should get the some thing like below..

\--- Checking Asterisk configuration to see if it will support the GUI --- \* Checking for http.conf: OK \* Checking for manager.conf: OK \* Checking if HTTP is enabled: OK \* Checking if HTTP static support is enabled: OK \* Checking if manager is enabled: OK \* Checking if manager over HTTP is enabled: OK --- Everything looks good --- **\* GUI should be available at https://aster:80/asterisk/static/config/index.html**

\* Note: If you have bindaddr=127.0.0.1 in /etc/asterisk/http.conf you will only be able to visit it from the local machine.

Example: http://localhost:80/asterisk/static/config/index.html

If you can not find the line " --- Everything looks good ---" , check the configuration changes and run the command again..

Open the browser and goto the url and login with username and password which you had added in manager.conf.

When you are login for the first time , it will take a while to load the default page. On first login GUI will check permission and take backup of asterisk configuration files before making changes.
