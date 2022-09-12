---
layout: post
title: Container Tips
categories: [ container, docker, podman ]
#image: "assets/images/2020/ansible-automation.png"
tags: [cloud, openshift, containers, kubernetes, devops]
permalink: container-tips
author: gini
featured: false
hidden: false
titleshort: container-tips
---

- [How to install `ping` utility inside container ?](#how-to-install-ping-utility-inside-container-)
- [How to allow continers to execute `ping` ?](#how-to-allow-continers-to-execute-ping-)
- [Error - "/" is not a shared mount](#error----is-not-a-shared-mount)

## How to install `ping` utility inside container ?

```shell
apt-get update
apt-get install iputils-ping

## fedora
dnf install iputils
```

## How to allow continers to execute `ping` ?

Run container with additional capabilities.

```shell
$ sudo podman run --cap-add net_raw --cap-add net_admin -it ubuntu bash
```

## Error - "/" is not a shared mount

ERROR on rootless mode

```shell
$ podman ps
WARN[0000] "/" is not a shared mount, this could cause issues or missing mounts with rootless containers
Error: cannot setup namespace using newuidmap: exit status 1
```

resolve the rootless mode problem

```
sudo chmod 4755 /usr/bin/newgidmap
sudo chmod 4755 /usr/bin/newuidmap
```
