---
layout: post
title: Wordpress with nginx

# 1. author: gini
categories: [ wordpress ]

# 2. image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: /wordpress-nginx
featured: false
hidden: false
titleshort: Wordpress nginx
---

<!-- TOC orderedList:true -->autoauto1. [1. author: gini](#1-author-gini)auto2. [2. image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"](#2-image-assetsimagesgini-redhat-cloudevent-2019-2jpg)auto3. [3. Install LEMP stack on Debian 10](#3-install-lemp-stack-on-debian-10)auto  1. [3.1. Set Up a Firewall with UFW on Debian](#31-set-up-a-firewall-with-ufw-on-debian)auto    1. [3.1.1. Install firewall - ufw](#311-install-firewall---ufw)auto    2. [3.1.2. Configure Firewall policy](#312-configure-firewall-policy)auto    3. [3.1.3. Enable ufw and check status](#313-enable-ufw-and-check-status)auto  2. [3.2. Install nginx](#32-install-nginx)auto  3. [3.3. Installing MariaDB](#33-installing-mariadb)auto  4. [3.4. Installing PHP](#34-installing-php)auto  5. [3.5. Configuring Nginx to Use the PHP Processor](#35-configuring-nginx-to-use-the-php-processor)auto    1. [3.5.1. Create your project directory](#351-create-your-project-directory)auto    2. [3.5.2. Create a configuration for your domain](#352-create-a-configuration-for-your-domain)auto    3. [3.5.3. Activate new configuration](#353-activate-new-configuration)auto    4. [3.5.4. Test config and reload nginx](#354-test-config-and-reload-nginx)auto    5. [3.5.5. Create Test php file](#355-create-test-php-file)auto  6. [3.6. Secure Nginx with Let's Encrypt on Debian 9](#36-secure-nginx-with-lets-encrypt-on-debian-9)auto    1. [3.6.1. Installing Certbot](#361-installing-certbot)auto    2. [3.6.2. Confirming Nginx’s Configuration](#362-confirming-nginxs-configuration)auto    3. [3.6.3. Verify and reload nginx config](#363-verify-and-reload-nginx-config)auto    4. [3.6.4. Obtaining an SSL Certificate](#364-obtaining-an-ssl-certificate)auto    5. [3.6.5. Verifying Certbot Auto-Renewal](#365-verifying-certbot-auto-renewal)auto4. [4. Configure Wordpress on LEMP Stack](#4-configure-wordpress-on-lemp-stack)auto  1. [4.1. Create Database and User](#41-create-database-and-user)auto  2. [4.2. Installing Additional PHP Extensions](#42-installing-additional-php-extensions)auto  3. [4.3. Configuring Nginx](#43-configuring-nginx)auto  4. [4.4. Downloading WordPress](#44-downloading-wordpress)auto  5. [4.5. Setting up the WordPress Configuration File](#45-setting-up-the-wordpress-configuration-file)auto  6. [4.6. Access the site and finish wordpress setup](#46-access-the-site-and-finish-wordpress-setup)auto5. [5. Migrating or Moving a Wordpress sire](#5-migrating-or-moving-a-wordpress-sire)auto  1. [5.1. Export Database](#51-export-database)auto  2. [5.2. Backup website files in zip file](#52-backup-website-files-in-zip-file)auto  3. [5.3. Copy databse and website backup to new server](#53-copy-databse-and-website-backup-to-new-server)auto  4. [5.4. Restore database on new server](#54-restore-database-on-new-server)auto  5. [5.5. Restore website files](#55-restore-website-files)auto  6. [5.6. Update and Verify your wp-config.php](#56-update-and-verify-your-wp-configphp)auto6. [6. Troubleshooting](#6-troubleshooting)auto  1. [6.1. How to fix 404 not found nginx problem?](#61-how-to-fix-404-not-found-nginx-problem)auto7. [7. Appendix:](#7-appendix)autoauto<!-- /TOC -->


# 3. Install LEMP stack on Debian 10
(Linux, Nginx, MariaDB, PHP)


## 3.1. Set Up a Firewall with UFW on Debian
[Reference](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-debian-9)

### 3.1.1. Install firewall - ufw
```
sudo apt install ufw
```

### 3.1.2. Configure Firewall policy

```
sudo ufw default deny incoming
sudo ufw default allow outgoing

# allow ssh, http, https
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

### 3.1.3. Enable ufw and check status

```
sudo ufw enable
sudo ufw status
```

**Resetting or Disabe ufw**
```
sudo ufw disable
# Any rules that you created with UFW will no longer be active. You can always run sudo ufw enable if you need to activate it later.

sudo ufw reset
# This will disable UFW and delete any rules that were previously defined. 
```

## 3.2. Install nginx
[Reference](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mariadb-php-lemp-stack-on-debian-10)
```
sudo apt update
sudo apt install nginx
```

## 3.3. Installing MariaDB
```
sudo apt install mariadb-server
```
When the installation is finished, it’s recommended that you run a security script that comes pre-installed with MariaDB. This script will remove some insecure default settings and lock down access to your database system. Start the interactive script by running:
```
sudo mysql_secure_installation
```

**And set root password.**

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MY_NEW_PASSWORD';
FLUSH PRIVILEGES;
```

**Login to mariadb**

```
$ sudo mariadb
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 61
Server version: 10.3.18-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

**Sample tests:**

```
# create database
MariaDB [(none)]> CREATE DATABASE example_database;

# create new user and grant access
MariaDB [(none)]> GRANT ALL ON example_database.* TO 'example_user'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;

# Flush the privileges to ensure that they are saved and available in the current session:
MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> exit;
```

## 3.4. Installing PHP
```
sudo apt install php-fpm php-mysql
```

## 3.5. Configuring Nginx to Use the PHP Processor

### 3.5.1. Create your project directory
```
sudo mkdir /var/www/your_domain
```

Assign ownership of the directory with the $USER environment variable, which should reference your current system user:
```
sudo chown -R $USER:$USER /var/www/wp-test
```

### 3.5.2. Create a configuration for your domain
```
sudo nano /etc/nginx/sites-available/wp-test

# Sample Content
server {
    listen 80;
    listen [::]:80;

    root /var/www/wp-test;
    index index.php index.html index.htm;

    server_name dev.yourdomain.com;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }
}
```

### 3.5.3. Activate new configuration

```
sudo ln -s /etc/nginx/sites-available/wp-test /etc/nginx/sites-enabled/
```

### 3.5.4. Test config and reload nginx
```
# test configuration
sudo nginx -t

# reload nginx
sudo systemctl reload nginx
```

### 3.5.5. Create Test php file

```
$ cat /var/www/wp-test/info.php
<?php
phpinfo();
```

## 3.6. Secure Nginx with Let's Encrypt on Debian 9

[Reference](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-debian-9)

### 3.6.1. Installing Certbot
To add the backports repository, first open `/etc/apt/sources.list` and add below lines.

```
deb http://deb.debian.org/debian stretch-backports main contrib non-free
deb-src http://deb.debian.org/debian stretch-backports main contrib non-free
```

Then,
```
sudo apt update
```

Install Certbot’s Nginx package with apt:
```
sudo apt install python-certbot-nginx -t stretch-backports
```

### 3.6.2. Confirming Nginx’s Configuration
Make sure your server block configuration contains correct `server_name` value for your domain.

```
$ sudo cat /etc/nginx/sites-available/wp-test |grep server_name 
    server_name dev.yourdomain.com;
```

### 3.6.3. Verify and reload nginx config
```
sudo nginx -t
sudo systemctl reload nginx
```
Certbot can now find the correct server block and update it.

### 3.6.4. Obtaining an SSL Certificate

```
sudo certbot --nginx -d dev.yourdomain.com -d mail.dev.yourdomain.com
```
It will ask for email address for notifications; also auto-redirect to https

### 3.6.5. Verifying Certbot Auto-Renewal

Test the renewal process, you can do a dry run with certbot:
```
sudo certbot renew --dry-run
```

# 4. Configure Wordpress on LEMP Stack

[Reference](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lemp-nginx-mariadb-and-php-on-debian-10)

## 4.1. Create Database and User
```
sudo mariadb -u root -p
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
GRANT ALL ON wordpress.* TO 'wordpress_user'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
exit;
```

## 4.2. Installing Additional PHP Extensions

```
sudo apt update
sudo apt install php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip
```

**Restart php-fpm process**
```
sudo systemctl restart php7.3-fpm.service
```

## 4.3. Configuring Nginx

Please ntoes, if you have enabled SSL (using Certbot) there will be two server blocks in your nginx configuration. Find the one with `root /var/www/your_domain` and add some entries as below.

```
server {
    . . .

    location = /favicon.ico { log_not_found off; access_log off; }
    location = /robots.txt { log_not_found off; access_log off; allow all; }
    location ~* \.(css|gif|ico|jpeg|jpg|js|png)$ {
        expires max;
        log_not_found off;
    }
    . . .
}
```

Also adjust `try_files`
```
    . . .
    location / {
        #try_files $uri $uri/ =404;
        try_files $uri $uri/ /index.php$is_args$args;
    }
    . . .
```

## 4.4. Downloading WordPress

```
cd /tmp
curl -LO https://wordpress.org/latest.tar.gz

# extract
tar xzvf latest.tar.gz

# copy sample wp-config.php
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php

# copy content to our domain location
sudo cp -a /tmp/wordpress/. /var/www/wp-test

# adjust permission
sudo chown -R www-data:www-data /var/www/wp-test
```

## 4.5. Setting up the WordPress Configuration File

**Generate secret keys and use it inside the config.**
```
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

Copy the content inside `wp-config.php` where you can find same entries with dummy values.

**Adjust database values**

Configure db entries as we generated in firt steps.
```
. . .

define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpress_user');

/** MySQL database password */
define('DB_PASSWORD', 'password');

. . .

define('FS_METHOD', 'direct');
```

## 4.6. Access the site and finish wordpress setup

Access the url over browser and complete wordpress configuration.

# 5. Migrating or Moving a Wordpress sire
Now we login to old server and export our database and website files (media, scripts, plugins etc)

## 5.1. Export Database
```
### from old server
$ mysqldump -u root -p old_databse |gzip > old_databse_20190104.gz
```

## 5.2. Backup website files in zip file
Let's gzip all web folder into a single file for easy transfer.

```
$ cd /var/www/
$ tar -cv html | gzip > mywpsite.tar.gz
```

## 5.3. Copy databse and website backup to new server
```
scp user@old_server:/home/user/old_databse_20190104.gz .
scp user@old_server:/home/user/mywpsite.tar.gz .
```

## 5.4. Restore database on new server
```
### on new server
gunzip < old_databse_20190104.gz  | mysql -u root -pMyPasswd wordpress101

```

## 5.5. Restore website files
```
### on new server
gunzip mywpsite.tar.gz
tar -xvf mywpsite.tar -C /web/pe/
```
Where -C is to point the destination directory.

## 5.6. Update and Verify your wp-config.php
Check your database name, username and password are properly configured.


# 6. Troubleshooting

## 6.1. How to fix 404 not found nginx problem?

Change the line in the location block to:

`try_files $uri $uri/ /index.php?q=$uri&$args;`

and reload nginx

`sudo systemctl reload nginx`

# 7. Appendix:
bitnami deployment
https://docs.bitnami.com/google/apps/wordpress-pro/configuration/create-vhost-nginx/

