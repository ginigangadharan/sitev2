---
layout: post
title: RHEL Upgrade - Quick Cheat Sheet
categories: [ Red Hat, Ansible, ]
tags: [ yum, redhat, satellite ]
show-avatar: false
permalink: rhel-upgrade
featured: false
hidden: false
titleshort: RHEL Upgarde
---


```shell
# Check current release
$ sudo subscription-manager release
Release: 9.1

# Check available releases
$ sudo subscription-manager release --list
+-------------------------------------------+
          Available Releases
+-------------------------------------------+
9
9.0
9.1
9.2
9.3
9.4
9.5

# Set the target release
$ sudo subscription-manager release --set 9.5
Release set to: 9.2

# Update all packages
$ sudo dnf update

# Reboot the system and check the version
```