---
layout: post
title: Linux Logical Volume Manager (LVM) Cheat Sheet
author: gini
categories: [ cheat sheets ]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: cheatsheet-lvm
featured: false
hidden: false
showindex: true
#titleshort: lvm cheatsheet
---



```shell
$ lvcreate -n NAME -l 100%FREE vg0
                                # Create a logical volume with full free size of vg0

```