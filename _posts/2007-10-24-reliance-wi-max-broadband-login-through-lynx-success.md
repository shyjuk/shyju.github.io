---
title: "Reliance Wi-Max Broadband Login Through Lynx-success"
date: 2007-10-24
categories: 
  - "technical"
---

Hi All,  
Here is my experiment with Reliance Wi-Max Broadband connection in  
Linux CLI.  
  
The wi-max connection only allow login in through http.  
So we first you need to create the login page(Find my worked example file)  
  
File named [form.htm](http://docs.google.com/View?docid=dfxvcb6_8hb4r3j)  
copy this file to your linux box.  
  
dont forget to change userid &password values and the permission of file(chmod )  
  
Install [Lynx](http://lynx.browser.org/) in Linux box if it is already installed.  
  
connect your Broadband connetion to LAN port.  
type /sbin/ifconfig  
if the connection is OK,you will get output some thing like this  
  
eth0 Link encap:Ethernet HWaddr 00:60:04:84:B1:D2  
inet addr:124.125.222.179 Bcast:124.125.223.255 Mask:255.255.254.0  
inet6 addr: fe80::250:4ff:fe74:c1d2/64 Scope:Link  
UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1  
RX packets:1500 errors:0 dropped:0 overruns:0 frame:0  
TX packets:7 errors:0 dropped:0 overruns:0 carrier:0  
collisions:0 txqueuelen:1000  
RX bytes:180049 (175.8 Kb) TX bytes:810 (810.0 b)  
Interrupt:20 Base address:0xe000  
  
If the connection is not OK you will not get the public IP.  
  
Then type the following command  
  
lynx -cmd\_log=login form.htm  
  
This will put you into Reliance Web Login Window  
Press down arrow key two times and then left arrow or enter key  
After login you can browse any sites,  
for this press g and enter URL like www.google.com and press enter.  
If it is working press q and y to exit from lynx browser.  
  
After exiting from lynx give the below command  
  
lynx -cmd\_script=login  
  
  
If it is not working edit the file "/etc/lynx.cfg"  
and find the line startting with STARTFILE:  
then change the line shown  
  
STARTFILE:file:/home/admin/form.htm  
(here admin is the current user form is the file which we have copied)  
  
  
Then we need to edit the crontab file .  
(If there is no network activity for some time the login session may terminate  
so that you need to relogin.This crontab file will run the script every 30 minute)  
Run these commands  
  
crontab -e  
press i for insert  
  
type  
30 \* \* \* \* lynx -cmd\_script=login  
  
  
type  
:wq  
  
for write and exit  
  
That is it..!!  
  
Now you have configured Reliance wi max connection which is always up..
