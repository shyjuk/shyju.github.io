---
title: "MySQL with PHP"
date: 2009-09-23
categories: 
  - "technical"
---

I done it  before but always  forget ..

Uzip the downloaded PHP zip file to C:\\

Rename the folder to PHP.

Take command prompt and change to C:\\PHP directory

Put the below commands.

```cmd
php.exe -d phar.require_hash=0 PEAR\go-pear.phar
pear install DB
```

Open php.ini-dist in notepad edit following lines and save it as  C:\\Windows\\php.ini

```ini
include_path = ".;c:\php\include;c:\php\pear"

extension_dir = "C:\PHP\ext"

display_errors = Off

extension=php_mysql.dll
```

MySQL should be installed and running..

Create `test.php`, put the following in it, and save it in your default web directory.

```php
<?php
phpinfo();
?>
```

restart apache from command line

```cmd
C:\Apache\bin>httpd.exe -k restart
```

go to  http://<your\_server>//test.php

Search for string "MySQL Support" .Check If it is there and enabled.

Done.. !!
