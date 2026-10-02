---
title: "MYSQL- PHP-Apache variable default value not defined prblm"
date: 2009-03-03
categories: 
  - "technical"
---

This is mysql sql-mode problem  
  
Change the following line in my.ini  
  
sql-mode="STRICT\_TRANS\_TABLES,NO\_AUTO\_CREATE\_USER,NO\_ENGINE\_SUBSTITUTION"  
  
to  
  
sql-mode="NO\_AUTO\_CREATE\_USER,NO\_ENGINE\_SUBSTITUTION"  
  

