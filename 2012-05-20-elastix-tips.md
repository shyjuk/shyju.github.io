---
title: "Elastix Tips"
date: 2012-05-20
categories: 
  - "technical"
tags: 
  - "assign-fax-device-to-user"
  - "assign-only-one-fax-device-for-each-elastix-user"
  - "asterisk"
  - "asterisk-fax-to-email"
  - "change-elastix-admin-password"
  - "change-elastix-admin-password-command"
  - "change-fax-from-name"
  - "change-from-address-asterisk-fax"
  - "elastix-dubai"
  - "elastix-show-only-one-fax-device-for-one-user"
  - "fax"
---

#### Change "From  Name"  in Elastix/FreePBX FAX to Email

Edit the file /var/lib/asterisk/bin/fax-process.pl

Change the line starting with "From: $from" to  "From: FAX Mailer \\<$from\\>" or put any name instead of FAX Mailer.

#### Assign only one Fax Device for each elastix user

Edit  the file /var/www/html/modules/sendfax/index.php

Comment the line starting with  $arrFaxList = array("none"=> and add the below two lines.

```php
// $arrFaxList = array("none" => '-- ' . $arrLang["Select a Fax Device"] . ' --');
$usrname = $_SESSION['elastix_user'];
exec("sqlite3 -separator '|' /var/www/db/acl.db \"select extension from acl_user where name='$usrname'\"", $user_exten);
```

Add the following line after the line starting with  "foreach($faxes as $values){"

if($\_SESSION\['elastix\_user'\] == "admin" || $user\_exten\[0\] == $values\['extension'\])

Edit  the file /var/www/html/modules/faxviewer/index.php

Add the following 2 lines after the line starting with  "if(is\_array($arrResult) && $total>0) {"

```php
$usrname = $_SESSION['elastix_user'];
exec("sqlite3 -separator '|' /var/www/db/acl.db \"select extension from acl_user where name='$usrname'\"", $user_exten);
```

Add the following line after the line starting with   " $fax\[$k\] = htmlentities($fax\[$k\], ENT\_COMPAT, 'UTF-8');"

 if($user\_exten\[0\] == $fax\['destiny\_fax'\] OR $\_SESSION\['elastix\_user'\] == "admin"){

Add a close the bracket after " "<a href='?menu=$module\_name&action=edit&id=".$fax\['id'\]."'>".\_tr('Edit')."</a>"); }"

**Change Elastix admin password**

```
sqlite3 /var/www/db/acl.db "UPDATE acl_user SET md5_password = '`echo -n password|md5sum|cut -d ' ' -f 1`' WHERE name = 'admin'"

Now Elastix admin password is 'password' (without quotes)
```

## Add a New Menu Item in Elastix / Issabel GUI

You want to publish a URL, example a database manager URL which you can access it with https://<elasitx\_ip>/dbadmin.

Your dbadmin is located under the folder /var/www/html/

  
then add the menu:  
\-------------------------------- 
`sqlite3 /var/www/db/menu.db`  
\-------------------------------- 
  
If you want add to the menu extras:  
\-------------------------------- 
`insert into menu values('dbadmin', 'extras', 'FOLDER', 'dbAdmin', 'frame','7');`  
\-------------------------------- 
FOLDER is the name of your dbadmin folder in /var/www/html  
  
then add the next:  
\-------------------------------- 
`sqlite3 /var/www/db/acl.db   insert into acl_resource values(124, 'dbAdmin', 'dbAdmin');`  
\-------------------------------- 
  
Note: See the first value (it's a number: 124), it can't be the same of another, first  
review the last id with:  
\-------------------------------- 
`select * from acl_resource;`  
\-------------------------------- 
and choose the last one +1  
  
Finally add:  
\-------------------------------- 
`insert into acl_group_permission values(166, 1, 1, 124);`  
\-------------------------------- 
Note: The first number (166) is:  
\-------------------------------- 
`select * from acl_group_permission;`  
\-------------------------------- 
the last one +1  
  
the last number (124) is the same that the inserted in the acl\_resource  
  
Now you can see the module dbAdmin in Elastix - Extras  
Remember that if you are logged you need logout and login.

Copied from new [elastix forum location](https://www.3cx.com/community/threads/how-to-add-a-menu-option.89895/)
