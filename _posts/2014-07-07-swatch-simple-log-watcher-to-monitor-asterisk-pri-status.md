---
title: "Swatch : Simple Log Watcher to monitor Asterisk PRI status"
date: 2014-07-07
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "check-and-run-command"
  - "restart-asterisk"
  - "restart-check"
  - "restart-dahdi"
  - "restart-on-log-change"
  - "swatch"
  - "voip-script"
---

This will monitor the asterisk log file and check for some pattern and restart the asterisk & dahdi service. ( can be used as a work around till you find the real problem).

- #### Install swatch
    

```
# yum install swatch
```

- **Create file daemon script**

/etc/init.d/swatch

```
#!/bin/sh
# Simple Log Watcher Program

case "$1" in
'start')
 /usr/bin/swatch --daemon --config-file=/etc/swatch.conf --tail-file=/var/log/asterisk/full --pid-file=/var/run/swatch.pid
 ;;
'stop')
 PID=`cat /var/run/swatch.pid`
 kill $PID
 ;;
*)
 echo "Usage: $0 { start | stop }"
 ;;
esac
exit 0

```

- #### Make it executable and enable autostart.
    

```
# sudo chmod 755 /etc/init.d/swatch
# sudo ln -s /etc/init.d/swatch /etc/rc2.d/S99swatch
# sudo ln -s /etc/init.d/swatch /etc/rc3.d/S99swatch
# sudo ln -s /etc/init.d/swatch /etc/rc5.d/S99swatch
```

- #### Create swatch config and commands.
    

vi /etc/swatch.conf

```
watchfor /Span 1: D-channel is down!|Primary D-Channel on span 1 down/
# echo
 write root
 mail addresses=user1\@mydomain.net:user2\@mydomain.net, subject=PRI down. Trying to restart.
 exec="/usr/bin/rstast.sh"
```

 

- #### Add commands in  script and make it executable.
    

vi /usr/bin/rstast.sh

```
#!/bin/sh
sudo asterisk -rx "logger rotate"
sleep 3
sudo /etc/init.d/asterisk stop
sleep 3
sudo /etc/init.d/dahdi restart
sleep 3
sudo /etc/init.d/asterisk start
sleep 2

# chmod 755 /usr/bin/rstast.sh
```
