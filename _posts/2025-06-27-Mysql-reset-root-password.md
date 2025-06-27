---
title: MySQL reset root passord
date: 2025-06-27
categories: [Linux, Tips, MySQL]
tags: [linux,tips,commands,mysql]     # TAG names should always be lowercase
---

Stop the current running MySQL Service

{% raw %}
```
sudo systemctl stop mysql

```
{% endraw %}

Create a run directory for mysql and give permission
{% raw %}
```
sudo mkdir -p /var/run/mysqld
sudo chown mysql:mysql /var/run/mysqld
```
{% endraw %}

Start the MySQL service in Safe mode
{% raw %}
```
sudo mysqld_safe --skip-grant-tables --skip-networking &

```
{% endraw %}

Login to mysql as root without password
{% raw %}
```
mysql -u root

```
{% endraw %}

Update the MySQL password to null
{% raw %}
```
UPDATE mysql.user SET authentication_string=null WHERE User='root';
FLUSH PRIVILEGES;
exit;
```
{% endraw %}

Login again and set the password
{% raw %}
```
mysql -u root

ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'yourpasswd';

```
{% endraw %}

Restart and try with new password 
