---
layout: post
title: RHEL 7/8 Kickstart Installation
author: gini
categories: [ redhat ]
image: "assets/images/2020/infrastructure.jpg"
tags: [infrastructure, rhel]
permalink: rhel-kickstart-installation
featured: false
hidden: false
titleshort: rhel-kickstart-installation
---


- [1. Pre-Requisites](#1-pre-requisites)
- [2. Prepare kickstart file](#2-prepare-kickstart-file)
- [3. Configure Utility Services](#3-configure-utility-services)
  - [3.1. Configure DHCP and DNS](#31-configure-dhcp-and-dns)
    - [Sample `dhcpd.conf`](#sample-dhcpdconf)
    - [Using DNSMASQ](#using-dnsmasq)
  - [3.2. Configure a Web Server](#32-configure-a-web-server)
  - [3.3. Download and Preapre RHEL ISO](#33-download-and-preapre-rhel-iso)
  - [3.4. Configure Repo Server and ISO](#34-configure-repo-server-and-iso)
  - [Setup a tftp server](#setup-a-tftp-server)
- [4. Setup a PXE Server](#4-setup-a-pxe-server)
  - [Configure Firewall](#configure-firewall)
- [5. Boot from ISO and Use kickstart configuration](#5-boot-from-iso-and-use-kickstart-configuration)
  - [5.1. Automated Booting and Installation](#51-automated-booting-and-installation)
- [6. Appendix](#6-appendix)
  - [6.1. References](#61-references)
  - [6.2. Validating Kickstart File - `ksvalidator`](#62-validating-kickstart-file---ksvalidator)
  - [6.3. Sample `ks.cfg` files](#63-sample-kscfg-files)

# 1. Pre-Requisites

# 2. Prepare kickstart file

- Generate kickstart file using [Kickstart Generator](https://access.redhat.com/labs/kickstartconfig/)
- Add/modify content as needed (like post-scripts, Software RAID etc)

# 3. Configure Utility Services

## 3.1. Configure DHCP and DNS

- Configure DHCP server in same network : We need a dhcp server to provide initial IP for the machine to use, so that it can access the http servers for ISO, Packages and `kickstart.cfg`
- Configure a DNS server in same network (Optional)

[dnsmasq](https://wiki.archlinux.org/index.php/dnsmasq)

### Sample `dhcpd.conf`

```
ddns-update-style none;
option domain-name "lab.local";
ignore client-updates;

authoritative;
allow booting;
allow bootp;
allow unknown-clients;

# internal subnet for my DHCP Server
subnet 10.1.10.0 netmask 255.255.255.0 {
range 10.1.10.161 10.1.10.170;
option domain-name-servers 10.1.10.120 1.1.1.1;
option routers 10.1.10.120;
option broadcast-address 10.1.10.255;
default-lease-time 600;
max-lease-time 7200;

# IP of PXE Server
next-server 10.1.10.120;
filename "pxelinux.0";
}
```
### Using DNSMASQ

```
# cat /etc/dnsmasq.conf |grep -v ^#|grep -v ^$
domain-needed
bogus-priv
server=1.1.1.1
server=8.8.8.8
address=/lab.local/127.0.0.1
address=/lab.local/10.1.10.120
interface=enp0s8,lo
listen-address=::1,127.0.0.1,10.1.10.120
bind-interfaces
expand-hosts
dhcp-range=10.1.10.150,10.1.10.160,255.255.255.0,12h
dhcp-option=3,10.1.10.254
dhcp-boot=pxelinux.0
enable-tftp
tftp-root=/tftpboot
pxe-prompt="Press F8 for menu.", 10
dhcp-leasefile=/var/lib/dnsmasq/dnsmasq.leases
dhcp-authoritative
domain=lab.local
conf-dir=/etc/dnsmasq.d,.rpmnew,.rpmsave,.rpmorig
```

[Setup pxeboot on dnsmasq](https://docs.oracle.com/en/operating-systems/oracle-linux/6/install/ol-dnsmasq-conf.html)

Note:
```
## check dnsmasq logs
## find below line in dnsmasq.conf
dhcp-leasefile=/var/lib/dnsmasq/dnsmasq.leases
```


## 3.2. Configure a Web Server

- To keep the ISO for booting and also to store YUM repository.
- If using PXE, then need to setp tftp server on same.
- It can be on same machine as DHCP server.

## 3.3. Download and Preapre RHEL ISO

Refer : [Product Downloads](https://access.redhat.com/downloads/)

- Download the minimal boot image ISO (eg: `Red Hat Enterprise Linux 8.2 Boot ISO`)
- Download the binary ISO and create a local repository server. 

## 3.4. Configure Repo Server and ISO

- Deploy a server with http (web server)
- Copy RHEL7/8 content to a directory on server and enable repo (Refer how to setup local repo server)
- Copy RHEL7/8 Boot ISO (minimal one) to another directory (if planning remote ISO loading)
- Copy kickstart to Web server path (eg: `/var/www/html/ks/rhel/8/ks.cfg`)

## Setup a tftp server

TFTP server
dnsmasq has built-in TFTP server.

To use it, create a root directory for TFTP (e.g. /srv/tftp) to put transferable files in.

enable-tftp
tftp-root=/srv/tftp
For increased security it is advised to use dnsmasq's TFTP secure mode. In secure mode only files owned by the dnsmasq user will be served over TFTP. You will need to chown TFTP root and all files in it to dnsmasq user to use this feature.

tftp-secure

# 4. Setup a PXE Server

```
# yum install dhcp tftp tftp-server syslinux vsftpd xinetd
# cp /usr/share/doc/dhcp-4.2.5/dhcpd.conf.example /etc/dhcp/dhcpd.conf
```

**Content of `/tftpboot/pxelinux.cfg/default`**
```
default menu.c32
prompt 0
timeout 300
ONTIMEOUT local

label 1
menu label ^1) Install RHEL 8 with Local Repo
kernel rhel8/vmlinuz
append initrd=rhel8/initrd.img method=http://10.1.10.120/rhel/8 inst.ks=http://10.1.10.120/ks/rhel/8/ks.cfg devfs=nomount
# append initrd=rhel8/initrd.img inst.ks=http://10.1.10.120/ks/rhel/8/ks.cfg devfs=nomount
label 2
menu label ^2) Boot from local drive
```

- [Refernce](https://www.linuxtechi.com/configure-pxe-installation-server-centos-7/)

## Configure Firewall

```
# firewall-cmd --add-service=ftp --permanent
success
# firewall-cmd --add-service=dhcp --permanent
success
# firewall-cmd --add-port=69/tcp --permanent 
success
# firewall-cmd --add-port=69/udp --permanent 
success
# firewall-cmd --add-port=4011/udp --permanent
success
# firewall-cmd --reload
```

# 5. Boot from ISO and Use kickstart configuration

- If we are loading ISO manually to VM/Physical servers, the boot from "RHEL7/8 Boot ISO" (either attach to VM or fetch it from http path which you have stored in repo server setup previously)
- When the ISO installer asks for "Install", press `Tab` and add `inst.ks=http://10.1.10.120/ks/rhel/8/ks.cfg` (your http path) and press `Enter`
- Installation will kickstart without asking for any answers (unless you missed anything in `ks.cfg`)

*Note : To load your Kickstart file automatically without having to specify the inst.ks= boot option, name the file ks.cfg and place it on a storage volume labeled `OEMDRV`.*

## 5.1. Automated Booting and Installation

- If you are planning fully automated installation, you can use `PXE` booting; it will load basic boot ISO (need to create custom boot images). 
- You can mention the custom `ks.cfg` in the boot command line itself and installation will not even ask for `kickstart` file or any answers.

***Warning**: Only use this method with restricted IP/DHCP/MAC binded network, otherwise it may wipe-out other systems when it reboot !!!*

# 6. Appendix

## 6.1. References

- [PREPARING INSTALLATION SOURCES](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-making-media-additional-sources)
- [HOW DO YOU PERFORM A KICKSTART INSTALLATION?](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-kickstart-howto)
- [PREPARING FOR A NETWORK INSTALLATION](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/chap-installation-server-setup)
- [RHEL/CentOS 8 Kickstart example](https://www.golinuxcloud.com/rhel-centos-8-kickstart-example-generator/)
- [SAMPLE KICKSTART CONFIGURATIONS](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-kickstart-examples)
  
## 6.2. Validating Kickstart File - `ksvalidator`

```
# yum install pykickstart
$ ksvalidator /path/to/kickstart.ks

## Changes in Kickstart Syntax
$ ksverdiff -f RHEL6 -t RHEL7
```

## 6.3. Sample `ks.cfg` files

- [kickstart basic sample](kickstart-sample-simple-ks.cfg)
- [kickstart with Software RAID1 + LVM](kickstart-sample-with-raid.cfg)
- [Install PXE Boot server for automated install RHEL 8 and CentOS 8](https://www.osradar.com/install-pxe-boot-server-for-automated-install-rhel-8-and-centos-8/)