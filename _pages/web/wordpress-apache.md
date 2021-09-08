---
layout: post
title: Wordpress with Apache
#author: gini
categories: [ wordpress ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: wordpress-apache
featured: false
hidden: false
titleshort: Wordpress Apache
---

Notes on Wordpress Site Migration

## Configure Your WebServer

### Setup a lamp deployment
You can do it manually or just create a template from deploymwent manager.
```
apt install apache2
apt install ufw
apt install mariadb-server
mysql_secure_installation
apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
```
### Enable the Rewrite Module
If you are setting up a fullstack lamp, this will be done automatically. But please make sure this is enabled.
(By this we can utilize the WordPress permalink feature)
```
sudo a2enmod rewrite
```

### Allowoverride
Enable .htaccess Overrides by editing the primary Apache configuration file.
To allow .htaccess files, we need to set the AllowOverride directive within a Directory block pointing to our document root. Towards the bottom of the file /etc/apache2/apache2.conf, add the following block:

```
<Directory /var/www/html/>
    AllowOverride All
</Directory>
```
(In our case I have used /web/mysite)

### Enable Virtualhost
This is to host multiple sites on same server and this is so easy.
Create a config file for your site (mysite.conf) under /etc/apache2/
```
<VirtualHost *:80>

ServerAdmin net.gini@gmail.com
ServerName mywebsite.com
ServerAlias www.mywebsite.com
DocumentRoot /web/mysite

ErrorLog ${APACHE_LOG_DIR}/mysite_error.log
CustomLog ${APACHE_LOG_DIR}/mysite_access.log combined

</VirtualHost>
```
#### Verify your configuration
```
sudo apache2ctl configtest
```

### Enable your website
This can be done at end as well, but you can do this now and check if site is working. And reload apache.
```
a2ensite mysite.conf
systemctl reload apache2
```

### Configure Let's Encrypt for SSL
```
## add below lines to /etc/apt/sources.list
deb http://ftp.debian.org/debian stretch-backports main

## update system
sudo apt update

## Install certbot
sudo apt install python-certbot-apache -t stretch-backports
## or 
sudo apt install python-certbot-apache

## Obtaining an SSL Certificate
sudo certbot --apache -d example.com -d www.example.com
```
### If Certbot is not working
```
wget https://dl.eff.org/certbot-auto && chmod a+x certbot-auto

cd /etc/letsencrypt/
cp ~/certbot-auto .

./certbot-auto 

```

### Install and configure firewall
Install ufw or firewalld (firewall) depends on the OS.
```
## install firewall
sudo apt install ufw

## check status
sudo ufw status

## List the ufw application profiles 
sudo ufw app list

## allow web traffic and ssh
sudo ufw allow 'WWW'
sudo ufw allow 'WWW Secure'
sudo ufw allow 'SSH'
sudo ufw allow 'OpenSSH'

## To allow all HTTP traffic you can remove WWW and add WWW Full
sudo ufw allow 'WWW Full'
sudo ufw delete allow 'WWW'

## enable ufw
sudo ufw enable

## check ufw status
sudo ufw status

```

## Create the directory for website
```
mkdir /web/mysite
chown www-data:www-data /web/mysite
```

## Configure Database
You need to create an admin user (for login purpose, instead of root), then a database for wordpress, an admin user for your wordpress database etc.

### Login to mysql and configure user
Login with root password
```
$ mysql -u root -pPASSWORD
mysql> 
```
### Create an Admin user 
This is to manange all databases udner mysql. (Instead of using root as login)
```
mysql> GRANT ALL ON *.* TO 'dbadmin'@'localhost' IDENTIFIED BY 'dbpassword' WITH GRANT OPTION;
```
### Reset root password
Update root user password and avoid default root password or blank root password.
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```

### Create a Databse for wordpress
You may use the same old database name from old server if you want to keep same documents.
```
mysql> CREATE DATABASE wordpress101 DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

### Create an admin user for wordpress database
Create a new user for wordpress. (If you want to keep the same old wordpress db username and password, use it here)
```
mysql> GRANT ALL ON wordpress101.* TO 'wpdbadmin'@'localhost' IDENTIFIED BY 'wpdbpassword';
```
### Update database system
You now have a database and user account, made specifically for WordPress. We need to flush the privileges so that the current instance of MySQL knows about the recent changes we've made.
```
mysql> FLUSH PRIVILEGES;
```

## Export your database and website files
Now we login to old server and export our database and website files (media, scripts, plugins etc)

### Export Database
```
### from old server
$ mysqldump -u root -p old_databse |gzip > old_databse_20190104.gz
```

### Backup website files in zip file
Let's gzip all web folder into a single file for easy transfer.
```
$ cd /var/www/
$ tar -cv html | gzip > mywpsite.tar.gz
```

### Copy databse and website backup to new server
```
scp user@old_server:/home/user/old_databse_20190104.gz .
scp user@old_server:/home/user/mywpsite.tar.gz .
```

### Restore database on new server
```
### on new server
gunzip < old_databse_20190104.gz  | mysql -u root -pMyPasswd wordpress101

```

### Restore website files
```
### on new server
gunzip mywpsite.tar.gz
tar -xvf mywpsite.tar -C /web/pe/
```
Where -C is to point the destination directory.

### Update and Verify your wp-config.php
Check your database name, username and password are properly configured.


