---
title: "Eucalyptus 4.0 web Console"
date: 2014-06-01
categories: 
  - "technical"
  - "virtualization"
tags: 
  - "bash-curl-ls-eucalyptus-com-install"
  - "centos-6-5"
  - "eucalyptus"
  - "the-one-line-install"
---

After the Eucalyptus FastStart  [one line](https://www.eucalyptus.com/eucalyptus-cloud/get-started) install run the following commands to create a web console user.

 

Run the command ".  ./eucarc "  from the root directory.

#.  ./eucarc

\[caption id="attachment\_661" align="alignleft" width="459"\][![eucarc,export source, euca enviornment](/images/eucarc.png)](/images/eucarc.png) source . ./eucarc\[/caption\]

 

 

 

 

 

 

 

 

 

Then create account. command "euare-accountcreate -a testuser1"

\# euare-accountcreate -a testuser1

\[caption id="attachment\_662" align="alignleft" width="620"\][![Create a Eucalyptus account.](/images/ac-create.png)](/images/ac-create.png) Create a Eucalyptus account.\[/caption\]

 

 

 

 

 

 

Create the admin user &  password and add the user to the account  using command "euare-useraddloginprofile --as-account testuser1 -u admin -p euca123"

\# euare-useraddloginprofile --as-account testuser1 -u admin -p euca123

[![Add user and attach the user to an account "euare-useraddloginprofile"](/images/login-profile.png)](/images/login-profile.png)

Add user and attach the user to an account

 

 

 

Create Key for the user.

\[caption id="attachment\_664" align="alignleft" width="620"\][![Create key for user](/images/useraddkey.png)](/images/useraddkey.png) Create key for user\[/caption\]

 

 

 

 

 

 

Login to Eucalyptus Web Console .

\[caption id="attachment\_666" align="alignleft" width="620"\][![Eucalyptus Web Console](/images/web-console.png)](/images/web-console.png) Eucalyptus Web Console\[/caption\]
