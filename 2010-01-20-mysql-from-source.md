---
title: "MySQL from Source"
date: 2010-01-20
categories: 
  - "technical"
tags: 
  - "m"
  - "mysql"
---

**Build and Install MySQL**

First, we create the group and user that "owns" MySQL. For security purposes, we don't want MySQL running as root on the system. To be able to easily identify MySQL processes in top or a ps list, we'll make a user and group named mysql:

groupadd mysql useradd -g mysql -c "MySQL Server" mysql

If you get any messages about the group or user already existing, that's fine. The goal is just to make sure we have them on the system.

What the useradd command is doing is creating a user mysql in the group mysql with the "name" of MySQL Server. This way when it's showed in various user and process watching apps, you'll be able to tell what it is right away.

Now we'll change to the "working" directory where the source code is, change the file 'ownership' for the source tree (this prevents build issues in reported in some cases where the packager's username was included on the source and you aren't using the exact same name to compile with!) and start building.

The configure command has many options you can specify. I have listed some fairly common ones; if you'd like to see others, do:

```bash
./configure --help | less
```

to see them all. Read the documentation on the MySQL website for a more detailed explanation of each option.

```bash
cd /usr/local/src/mysql-4.1.22
```

```bash
chown -R root.root *
```

```bash
make clean
```

```bash
./configure \\
  --prefix=/usr/local/mysql \\
  --localstatedir=/usr/local/mysql/data \\
  --disable-maintainer-mode \\
  --with-mysqld-user=mysql \\
  --with-unix-socket-path=/tmp/mysql.sock \\
  --without-comment \\
  --without-debug \\
  --without-bench
```

18-Jul-2005: If you are installing MySQL 4.0.x on Fedora Core 4, there is a problem with LinuxThreads that prevents MySQL from compiling properly. Installing on Fedora Core 3 works fine though. Thanks to Kevin Spencer for bringing this to my attention. There is a workaround listed at http://bugs.mysql.com/bug.php?id=9497. Thanks to Collin Campbell for that link. Another solution can be found at http://bugs.mysql.com/bug.php?id=2173. Thanks to Kaloyan Raev for that one.

Now comes the long part, where the source code is actually compiled and then installed. Plan to get some coffee or take a break while this step runs. It could be 10-15 minutes or more, depending on your system's free memory, load average, etc.

```bash
make && make install
```

**Configure MySQL**

MySQL is "installed" but we have a few more steps until it's actually "done" and ready to start. First run the script which actually sets up MySQL's internal database (named, oddly enough, mysql).

```bash
./scripts/mysql_install_db
```

Then we want to set the proper ownership for the MySQL directories and data files, so that only MySQL (and root) can do anything with them.

```bash
chown -R root:mysql /usr/local/mysql
chown -R mysql:mysql /usr/local/mysql/data
```

Copy the default configuration file for the expected size of the database (small, medium, large, huge)

```bash
cp support-files/my-medium.cnf /etc/my.cnf
chown root:sys /etc/my.cnf
chmod 644 /etc/my.cnf
```

If you get an error message about the data directory not existing, etc., something went wrong in the mysql\_install\_db step above. Go back and review that; make sure you didn't get some sort of error message when you ran it, etc.

Now we have to tell the system where to find some of the dynamic libraries that MySQL will need to run. We use dynamic libraries instead of static to keep the memory usage of the MySQL program itself to a minimum.

```bash
echo "/usr/local/mysql/lib/mysql" >> /etc/ld.so.conf
ldconfig
```

Now create a startup script, which enables MySQL auto-start each time your server is restarted.

```bash
cp ./support-files/mysql.server /etc/rc.d/init.d/mysql
chmod +x /etc/rc.d/init.d/mysql
/sbin/chkconfig --level 3 mysql on
```

Then set up symlinks for all the MySQL binaries, so they can be run from anyplace without having to include/specify long paths, etc.

```bash
cd /usr/local/mysql/bin
for file in *; do ln -s /usr/local/mysql/bin/$file /usr/bin/$file; done
```

[Link](http://www.lamphowto.com/)

**Remote Access Database** GRANT ALL ON asterisk.\* TO asterisk@'192.168.20.117' IDENTIFIED BY 'password';

The above command will grand access to db asterisk by user asterisk from ip 192.168.20.117
