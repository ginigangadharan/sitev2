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
  - [1.1. Configure DHCP and DNS](#11-configure-dhcp-and-dns)
- [2. Download and Preapre RHEL ISO](#2-download-and-preapre-rhel-iso)
- [3. Prepare kickstart file](#3-prepare-kickstart-file)
- [4. Configure Repo Server and ISO](#4-configure-repo-server-and-iso)
- [5. Boot from ISO and Use kickstart configuration](#5-boot-from-iso-and-use-kickstart-configuration)
  - [5.1. Automated Booting and Installation](#51-automated-booting-and-installation)
- [6. Appendix](#6-appendix)
  - [6.1. References](#61-references)
  - [6.2. Maintaining the Kickstart File](#62-maintaining-the-kickstart-file)
    - [6.2.1. Changes in Kickstart Syntax](#621-changes-in-kickstart-syntax)

# 1. Pre-Requisites

## 1.1. Configure DHCP and DNS

- Configure DHCP server in same network : We need a dhcp server to provide initial IP for the machine to use, so that it wasn access the http servers for ISO, Packages and `kickstart.cfg`
- Configure a DNS server in same network (Optional)

# 2. Download and Preapre RHEL ISO

Refer : [Product Downloads](https://access.redhat.com/downloads/)

- Download the minimal boot image ISO (eg: `Red Hat Enterprise Linux 8.2 Boot ISO`)
- Download the binary ISO to copy to repository

# 3. Prepare kickstart file

- Generate kickstart file using [Kickstart Generator](https://access.redhat.com/labs/kickstartconfig/)
- Add/modify content as needed (like post-scripts, Software RAID etc)

# 4. Configure Repo Server and ISO

- Deploy a server with http
- Copy RHEL7/8 content to a directory on server and enable repo (Refer how to setup local repo server)
- Copy RHEL7/8 Boot ISO (minimal one) to another directory (if planning remote ISO loading)
- Copy kickstart to Web server path (eg: `/var/www/html/ks/rhel/8/ks.cfg`)

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

## 6.2. Maintaining the Kickstart File

```
# yum install pykickstart
$ ksvalidator /path/to/kickstart.ks
```

### 6.2.1. Changes in Kickstart Syntax

```
$ ksverdiff -f RHEL6 -t RHEL7
```

