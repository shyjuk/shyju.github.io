---
title: "MySQLdb Install"
date: 2009-07-23
categories: 
  - "technical"
---

Re: MySQLdb Install Linux Not Working (SOLVED) Posted by: Kharlamov Igor () Date: January 29, 2008 09:33AM

I solved the problem by next way:

1\. Setup MySQL (version 4.1.22 or 5.0.51) by default. Location is /usr/local/mysql. 2. Check up file existing:

/usl/local/mysql/lib/mysql:

\[zope@localhost mysql\]$ ll total 2640 -rw-r--r-- 1 root mysql 20842 Jan 29 14:05 libdbug.a -rw-r--r-- 1 root mysql 70152 Jan 29 14:05 libheap.a -rw-r--r-- 1 root mysql 22754 Jan 29 14:05 libmerge.a -rw-r--r-- 1 root mysql 460804 Jan 29 14:05 libmyisam.a -rw-r--r-- 1 root mysql 41762 Jan 29 14:05 libmyisammrg.a -rw-r--r-- 1 root mysql 682096 Jan 29 14:05 libmysqlclient.a -rwxr-xr-x 1 root mysql 879 Jan 29 14:05 libmysqlclient.la lrwxrwxrwx 1 root mysql 24 Jan 29 14:05 libmysqlclient.so -> libmysqlclient.so.14.0.0 lrwxrwxrwx 1 root mysql 24 Jan 29 14:05 libmysqlclient.so.14 -> libmysqlclient.so.14.0.0 -rwxr-xr-x 1 root mysql 465740 Jan 29 14:05 libmysqlclient.so.14.0.0 -rw-r--r-- 1 root mysql 313436 Jan 29 14:05 libmystrings.a -rw-r--r-- 1 root mysql 399512 Jan 29 14:05 libmysys.a -rw-r--r-- 1 root mysql 158668 Jan 29 14:05 libnisam.a -rw-r--r-- 1 root mysql 9412 Jan 29 14:05 libvio.a

2\. Start MySQL and check up mysqladmin ping, should be:

\[zope@localhost bin\]$ ./mysqladmin ping mysqld is alive

3\. Before install MySQL-python set LD\_LIBRARY\_PATH, the best is to add into ./bash\_profile or into ldconfig:

$ LD\_LIBRARY\_PATH=/usr/local/mysql/lib/mysql $ export LD\_LIBRARY\_PATH

4\. I've tried to implement MySQL-python-1.2.1\_p2:

$ tar zxvf MySQL-python-1.2.1\_p2 $ cd MySQL-python-1.2.1\_p2

step1 (note, all is False): in site.cfg set: \[options\] embedded = False threadsafe = False static = False

run: $ python2.3 setup.py build

should be without errors.

step2: in site.cfg set again, but now threadsafe is True: \[options\] embedded = False threadsafe = True static = False

run one more: $ python2.3 setup.py build

5\. Install (LD\_LIBRARY\_PATH must be the same):

\# python2.3 setup.py install

6\. Check up the results:

\[zope@localhost MySQL-python-1.2.1\_p2\]$ python2.3 Python 2.3.6 (#1, Dec 28 2007, 12:39:17) \[GCC 4.1.1 20070105 (Red Hat 4.1.1-52)\] on linux2 Type "help", "copyright", "credits" or "license" for more information. >>> import MySQLdb >>>

should be without errors!

7\. That's all, ready to use. I can implement MySQL under zope, for instance. Is running. Good.

But, i don't know exactly will it work with threadsafe ;) Sorry.

Edited 1 time(s). Last edit at 01/29/2008 09:35AM by Kharlamov Igor.
