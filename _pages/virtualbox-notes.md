---
layout: post
title: VirtualBox Notes
categories: [ openshift ]
tags: [ OpenShift Installation ]
show-avatar: false
permalink: /Virtualbox
featured: false
hidden: false
titleshort: Virtualbox
---

- [1. VirtualBox Guest Addtion on CLI](#1-virtualbox-guest-addtion-on-cli)
- [2. Troubleshooing VirtualBox VM](#2-troubleshooing-virtualbox-vm)
  - [2.1. Unable to mount Shared folders in CLI](#21-unable-to-mount-shared-folders-in-cli)

# 1. VirtualBox Guest Addtion on CLI

- Mount Guest Addtion ISO (from Console Menu)
- Mount it from OS (/vmboxguestiso is destination mount point)
```
mount /dev/cdrom /vmboxguestiso/
```
- Resolve Dependancies

**Install kernel-header**

```
`sudo yum update && sudo yum -y install kernel-headers kernel-devel`

## install other packages
yum install gcc make perl
```

# 2. Troubleshooing VirtualBox VM

## 2.1. Unable to mount Shared folders in CLI

Check vboxsf module 
`sudo modprobe vboxsf`
