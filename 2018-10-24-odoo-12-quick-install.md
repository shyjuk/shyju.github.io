---
title: "Odoo 12 on Ubuntu 20.04.1 LTS"
date: 2018-10-24
categories: 
  - "technical"
tags: 
  - "active-directory"
  - "ldap"
  - "odoo"
  - "odoo-active-directory-integration"
  - "odoo-installation"
  - "security-group"
---

1. Login as root  
    `sudo su`

3. Install the following dependency libraries.  
    `apt install postgresql python3-pip python3-ldap python3-psycopg2 fontconfig libjpeg-turbo8 xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils node-less npm git` `libsasl2-dev python3-dev libldap2-dev` `libssl-dev` libxml2-dev libxslt-dev  
      
    **Install Less Plugin**  
    `npm install -g less less-plugin-clean-css`  
    **Install pip requirements**  
    `pip3 install chardet decorator docutils ebaysdk feedparser gevent greenlet html2text Jinja2 libsass lxml Mako MarkupSafe mock num2words ofxparse passlib Pillow psutil pydot pyparsing PyPDF2 pyserial python-dateutil pytz pyusb PyYAML qrcode reportlab requests suds-jurko vatnumber vobject Werkzeug XlsxWriter xlwt xlrd`  
    **Install wkhtmltox**  
    `wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb`  
    `dpkg -i wkhtmltox_0.12.6-1.focal_amd64.deb`  
    ```bash
    sudo apt -f install
    ```
    `ln -s /usr/local/bin/wkhtmltopdf /usr/bin`  
    `ln -s /usr/local/bin/wkhtmltoimage /usr/bin`

5. Create odoo user  
    `adduser --system --home=/opt/odoo --group odoo`

7. Create postgre user for odoo  
    `sudo su postgres   createuser --createdb --username postgres --no-createrole --no-superuser odoo`  
    `exit`

9. Download odoo source  
    `sudo su - odoo -s /bin/bash`  
    `cd /opt/   git clone https://www.github.com/odoo/odoo --depth 1 --branch 12.0 --single-branch`  
    Create custom addon folder for your custom addons  
    `mkdir /opt/odoo/custom_addons`  
    `exit`

11. Copy odoo.conf file add addon paths  
     `cp /opt/odoo/debian/odoo.conf /etc/odoo.conf`  
     `echo "addons_path = /opt/odoo/odoo/addons`,`/opt/odoo/addons,/opt/odoo/custom_addons" >> /etc/odoo.conf`  
     `echo "logfile = /var/log/odoo/odoo.log" >> /etc/odoo.conf`  
     `chown odoo /etc/odoo.conf`

13. Create log folder  
     `mkdir /var/log/odoo   chown odoo /var/log/odoo`

15. Create startup script and enable  
     vi /etc/systemd/system/odoo.service  
     `[Unit]`  
     `Description=Odoo`  
     `Documentation=http://www.odoo.com`  
     `[Service]`  
     `#Ubuntu/Debian convention:`  
     `Type=simple`  
     `User=odoo`  
     `ExecStart=/opt/odoo/odoo-bin -c /etc/odoo.conf`  
     `[Install]`  
     `WantedBy=default.target`  
     _type ":x" to write and exit_  
     `systemctl enable odoo   systemctl start odoo   systemctl status odoo`

17. Navigate to the odoo URL from the browser  
     http://<odoo-server-ip>:8069/

19. If you face any issues check the log file.  
     `tail -f /var/log/odoo/odoo.log`

## Odoo Active Directory Integration

See the LDAP filter section. I have restricted user login only to the Active Directory security group "MyGroup" by adding the memberOf section.

[![](/images/image.png)](/images/image.png)
