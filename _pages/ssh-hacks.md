---
layout: post
title: SSH Hacks and Tips
author: gini
categories: [ Linux ]
tags: [cloud, automation, containers, kubernetes]
permalink: ssh-hacks
featured: false
hidden: false
image: "assets/images/tools-1426927308491-6380b6a9936f.jpeg"
showindex: true
---
- [SSH Login to Remote Server over Jumphost](#ssh-login-to-remote-server-over-jumphost)
- [NFS over SSH Tunnel](#nfs-over-ssh-tunnel)
- [Install python3 offline - Centos](#install-python3-offline---centos)


## SSH Login to Remote Server over Jumphost

`ssh -J user@JUMP_HOST user@TARGET_HOST`

Copy:
`scp -o 'ProxyJump user@JUMP_HOST' FILE_OR_DIR user@TARGET_HOST:/PATH_TO_DIR`

## NFS over SSH Tunnel
Ref: [Gist](https://gist.github.com/proudlygeek/5721498)

```
mkdir /mnt/nfs-share

#-- Start SSH tunnel (local-to-remote port)
ssh -fNv -L 3049:localhost:2049 user@JUMP_HOST

#-- Mount the folder
mount -t nfs -o port=3049 localhost:/PATH_TO_DIR /mnt/MOUNT_PATH
```
    
## Install python3 offline - Centos
- [Python3 on CentOS Offline](https://medium.com/@CurtisForrester/installing-python3-in-centos-7-6-offline-69d45ca48054)
- [Another one](https://7thzero.com/blog/preparing-an-offline-installation-of-python-3-4-packages-for-centos-6)