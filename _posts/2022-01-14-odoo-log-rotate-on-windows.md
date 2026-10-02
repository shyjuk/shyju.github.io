---
title: "Odoo log rotate - on Windows"
date: 2022-01-14
categories: 
  - "technical"
---

Don't use this script if you can not afford a odoo service restart

Put the below script in a batch file and create a scheduled task in [Windows Task Scheduler](https://stackoverflow.com/questions/4437701/run-a-batch-file-with-windows-task-scheduler) (preferably nighlty) to run it.

```
net stop odoo-server-12.0
move E:\odoo\logs\odoo.log E:\odoo\logs\odoo-%DATE:~10,4%%DATE:~4,2%%DATE:~7,2%.log
net start odoo-server-12.0

forfiles /p "E:\odoo\logs" /s /d /m *.log -20 /c "cmd /c del @file"
```
