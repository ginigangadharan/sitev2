---
layout: post
title: Red Hat Knowledge Base
categories: [ Red Hat, Ansible, OpenShift, OpenStack ]
tags: []
show-avatar: false
permalink: /redhat-kb
featured: false
hidden: false
showindex: true
---

Reference Documents for Red Hat Products

<!-- TOC -->

- [1. Subscription](#1-subscription)
  - [1.1. Subscription Reference](#11-subscription-reference)
- [2. Repository & Satellite](#2-repository--satellite)
- [3. Useful Links](#3-useful-links)

<!-- /TOC -->

# 1. Subscription

```
subscription-manager register
subscription-manager register --username <username> --password <password> --auto-attach
subscription-manager refresh
subscription-manager attach --auto

# Unregistering a system
subscription-manager remove --all
subscription-manager unregister
subscription-manager clean
```

## 1.1. Subscription Reference

- [How to register and subscribe a system offline to the Red Hat Customer Portal](https://access.redhat.com/solutions/3121571)
- [How to register and subscribe a system to the Red Hat Customer Portal using Red Hat Subscription-Manager](https://access.redhat.com/solutions/253273)
- [Common administrative commands in Red Hat Enterprise Linux 5, 6, 7, and 8](https://access.redhat.com/articles/1189123) (Subscription Details, RHN ID, UUID)

# 2. Repository & Satellite

- [How to create a local mirror of the latest update for Red Hat Enterprise Linux 5, 6, 7, 8 without using Satellite server?](https://access.redhat.com/solutions/23016)

- [Creating a Local Repository and Sharing With Disconnected/Offline/Air-gapped Systems [Master Article]](https://access.redhat.com/solutions/3176811)

- [Yum Repository Configuration Helper](https://access.redhat.com/labs/yumrepoconfighelper/)

  This tool will help you set up a simple Yum repository for your local machine or a small number of other machines to use. 
  
- [How to create local repository distributed through apache of Red Hat Enterprise Linux 5/6/7/8 using DVD iso for update or installation?](https://access.redhat.com/solutions/7227)

# 3. Useful Links

- [Red Hat on GitHub](https://redhatofficial.github.io/)