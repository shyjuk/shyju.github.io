---
title: "Logrotate odoo for Windows"
date: 2023-02-23
categories: 
  - "technical"
---

net stop odoo-server-12.0  
move D:\\odoo\\logs\\odoo.log D:\\odoo\\logs\\odoo-%DATE:~10,4%%DATE:~4,2%%DATE:~7,2%.log  
net start odoo-server-12.0

%SYSTEMROOT%\\System32\\forfiles.exe -p "D:\\odoo\\logs" -s -m \*.log -d -31 -c "cmd /c del @path"
