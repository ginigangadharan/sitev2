---
layout: post
title: Remote Access without VPN - using Apache Guacamole
categories: [ utils ]
tags: []
show-avatar: false
permalink: apache-guacamole
featured: false
hidden: false
---

## Download Package and extract

https://guacamole.apache.org/releases/1.1.0/

```
$ tar -xzf guacamole-server-1.1.0.tar.gz
$ cd guacamole-server-1.1.0/
```

## Install Pre-Req

Install needed packages

```
sudo apt-get install libpng-dev
sudo apt-get install -y libjpeg-dev
sudo apt-get install libcairo-dev
sudo apt-get install libiossp-uuid-dev
  freerdp2-dev
  
```

## Configure, Make and install

Make sure all required options are enabled with packages

```
$ ./configure --with-init-dir=/etc/init.d
$ make
$ make install
$ ldconfig
```


# Install Client 

## Download and extract
```
tar -zxf guacamole-client-1.1.0.tar.gz
cd guacamole-client-1.1.0/

sudo apt install maven

mv package
```


