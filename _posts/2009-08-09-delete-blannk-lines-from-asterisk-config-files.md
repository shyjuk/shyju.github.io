---
title: "Delete Blank lines from Asterisk Config files"
date: 2009-08-09
categories: 
  - "asterisk"
  - "technical"
  - "voip"
tags: 
  - "asterisk-gui"
  - "blank-lines"
  - "configuration-files"
  - "internet-telephony"
  - "pbx"
---

When I installed the Asterisk-GUI., I found that once we make any changes in configuration from i asterisk-gui, it will insert many blank lines along with the changes in the configuration files(/etc/asterisk/\*.conf).. So no. of the blank lines increases with configuration changes and after certain no of changes the the asterisk pbx stop responding and it could not read the config files because of the huge file size..

I found a work around for this problem by deleting the blank lines when the config file changes.. For this we have to make the below change in asteisk-gui file "/var/lib/asterisk/static-http/config/js/index.js". Search for "module reload".Then put the following lines after that like below ..

var t = ASTGUI.cliCommand('module reload'); } **ASTGUI.systemCmdWithOutput("/bin/sed '/^$/d' -i /etc/asterisk/users.conf",function(a1){}); ASTGUI.systemCmdWithOutput("/bin/sed '/^$/d' -i /etc/asterisk/extensions.conf",function(b1){});** u.style.display = 'none'; top.cookies.remove('configFilesChanged');

This will remove blank lines only from users.conf&extensions.conf(these are main files where the configuration changes happens repeatedly)
