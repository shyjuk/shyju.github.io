---
title: "Disable / Enable Elstatix GUI (httpd) from your phone: Asterisk Dialplan"
date: 2015-06-18
categories: 
  - "technical"
tags: 
  - "asterisk"
  - "dial-to-and-start-server"
  - "dial-to-to-stop-server"
  - "dialplan-to-stop-httpd"
  - "elastix-gui"
  - "stop-start-httpd"
---

1. Create 2 scripts to start & stop httpd daemon.
    1. httpstop
        
        ```
        #!/usr/bin/php
        <?php
        $output = $ret = NULL;
        exec("/sbin/service httpd stop 2>&1", $output, $ret);
        ?>
        ```
        
    2. httpstart
        
        ```
        #!/usr/bin/php
        <?php
        $output = $ret = NULL;
        exec("/sbin/service httpd start 2>&1", $output, $ret);
        ?>
        ```
        
2. Make them executable (chmod +x) and copy to /usr/share/elastix/privileged
3. Edit the file /etc/asterisk/extensions\_custom.conf and add the lines under context \[from-internal-custom\].
    
    ```
    [from-internal-custom]
    exten => 120,1,Playback(posix-restarting) ; "Stop HTTP Server"
    exten => 120,2,Wait(1)
    exten => 120,3,System(sudo /usr/sbin/elastix-helper httpstop)
    exten => 120,4,Playback(disabled)
    exten => 120,5,Hangup
    
    exten => 121,1,Playback(posix-restarting) ; "Start HTTP server"
    exten => 121,2,Wait(1)
    exten => 121,3,System(sudo /usr/sbin/elastix-helper httpstart)
    exten => 121,4,Playback(enabled)
    exten => 121,5,Hangup
    ```
    
    I used the number 120 to stop the web server & 121 to start it.
4. Reload asterisk and dial 120 & 121 to test.
5. If you want to dial it from outside add above numbers in "Misc Destination" menu and then select them from IVR menu.
