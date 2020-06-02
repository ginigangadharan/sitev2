---
layout: post
title: Laravel Playground
categories: [ cloud ]
tags: []
show-avatar: false
permalink: php-laravel
featured: false
hidden: false
---

## Setup Laravel Homestead
```
# download box
vagrant box add laravel/homestead

# if error try below
vagrant box add laravel/homestead https://atlas.hashicorp.com/laravel/boxes/homestead

```

## Install Homestead

mkdir ~/Homestead
git clone https://github.com/laravel/homestead.git ~/Homestead

cd ~/Homestead
git checkout release


// Mac / Linux...
bash init.sh

// Windows...
init.bat


## Environment Variables

```
$ cat Homestead.yaml 
---
ip: "192.168.10.10"
memory: 2048
cpus: 2
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/code
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/public

databases:
    - homestead

features:
    - mariadb: false
    - ohmyzsh: false
    - webdriver: false

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```


```
vagrant up
```


## Create new site

```
laravel new mysite
```

# Edit content

## View
Resources -> Views > Welcome-blade.php
 change basic content

## Enable Auth

```
php artisan make:auth
```
if error, do below as got changes in version 6

```
composer require laravel/ui
php artisan ui vue --auth
npm install && npm run dev
php artisan migrate
```

## Check npm

vagrant@homestead:~/wnyh-uat$ npm -v
6.14.3

## create database

using sqlite for demo
`$ vim database/database.sqlite` and exit

then edit `.env` file and config
```
DB_CONNECTION=sqlite
#DB_HOST=127.0.0.1
#DB_PORT=3306
#DB_DATABASE=laravel
#DB_USERNAME=root
#DB_PASSWORD=
```

run migrate

```
$ php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table (0.02 seconds)
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table (0.01 seconds)
Migrating: 2019_08_19_000000_create_failed_jobs_table
Migrated:  2019_08_19_000000_create_failed_jobs_table (0 seconds)
```

check app and try to register and login

## Change Title and Layout

Resources -> Views > Layouts -> app.blade.php


## change logo
create a dir in Public/svg and add logo file
change app.blade.php
add "d-flex" for div to make it side by side

```
<a class="navbar-brand d-flex" href="{{ url('/') }}">
    <div><img src="/svg/logo.svg" style="height: 25px"></div>
    <div>WNYH.com</div>
</a>
```

## add content to home

Resources -> view -> home.blade.app.



# Troubleshooting

## 500 - Server Error

```
vagrant@homestead:~/wnyh-uat$ php artisan config:clear
Configuration cache cleared!
vagrant@homestead:~/wnyh-uat$ php artisan clear-compiled
Compiled services and packages files removed!
vagrant@homestead:~/wnyh-uat$ php artisan optimize
Configuration cache cleared!
Configuration cached successfully!
Route cache cleared!
```

find ~/wnyh-uat/ -type f -exec chmod 644 {} \;
find ~/wnyh-uat/ -type d -exec chmod 755 {} \;

chown -R www-data:www-data ~/wnyh-uat/

chgrp -R www-data storage bootstrap/cache
chmod -R ug+rwx storage bootstrap/cache

