---
layout: post
title: Red Hat Knowledge Base
categories: [ Red Hat, Ansible, OpenShift, OpenStack ]
tags: [ yum, redhat, satellite, ansible, openshift ]
show-avatar: false
permalink: redhat-kb
featured: false
hidden: false
titleshort: Red Hat KB
---

Reference Documents for Red Hat Products

- [sosreport methods](#sosreport-methods)
    - [Subscription Reference](#subscription-reference)
  - [Repository \& Satellite](#repository--satellite)
  - [Useful Links](#useful-links)
  - [Appendix](#appendix)

## Red Hat Subscription Management Cheatsheet

Refer [SUBSCRIPTION-MANAGER COMMAND CHEAT SHEET](https://access.redhat.com/sites/default/files/attachments/rh_sm_command_cheatsheet_1214_jcs_print.pdf)

```shell
# subscribe
subscription-manager register

# subscribe using username and password in line and auto-attach pool
subscription-manager register --username <username> --password <password> --auto-attach

# Pull the latest subscription data from the server
subscription-manager refresh

# Set a role for your system.
subscription-manager role --set="Red Hat Enterprise Linux Server"

# List available subscriptions
subscription-manager list --available

# attach subscription automatically with availalbe pool
subscription-manager attach --auto

# Use the pool ID of the subscription to attach the pool to the system.
subscription-manager attach --pool=<subscription-pool-id>

# check consumed subscrptions
subscription-manager list --consumed

# Subscription details
subscription-manager identity
subscription-manager status

# listing repositories
subscription-manager repos --list

# enabling server repos
subscription-manager repos \
  --enable=rhel-7-server-rpms \
  --enable=rhel-server-rhscl-7-2.9.rpms

# or for RHEL8
subscription-manager repos \
  --enable=rhel-8-for-x86_64-baseos-rpms \
  --enable=rhel-8-for-x86_64-appstream-rpms

# enabling ansible repo
subscription-manager repos \
  --enable ansible-2-for-rhel-8-x86_64-rpms

# enabling AAP repo
subscription-manager repos \
  ansible-automation-platform-2.2-for-rhel-8-x86_64-rpms

# Unregistering a system
subscription-manager remove --all
subscription-manager unregister
subscription-manager clean
```

**YUM Download only**

```shell
yum install --downloadonly --downloaddir=<directory> <package>
```


sosreport methods
========================

```shell
# Generate report to a specific path
sosreport -k docker.all=on -k docker.logs=on --tmp-dir /root/sos
```

- [How to provide files to Red Hat Support (vmcore, rhev logcollector, sosreports, heap dumps, log files, etc.)](https://access.redhat.com/solutions/2112)
### Subscription Reference

- [How to register and subscribe a system offline to the Red Hat Customer Portal](https://access.redhat.com/solutions/3121571)
- [How to register and subscribe a system to the Red Hat Customer Portal using Red Hat Subscription-Manager](https://access.redhat.com/solutions/253273)
- [Common administrative commands in Red Hat Enterprise Linux 5, 6, 7, and 8](https://access.redhat.com/articles/1189123) (Subscription Details, RHN ID, UUID)

## Repository & Satellite

- [How to create a local mirror of the latest update for Red Hat Enterprise Linux 5, 6, 7, 8 without using Satellite server?](https://access.redhat.com/solutions/23016)

- [Creating a Local Repository and Sharing With Disconnected/Offline/Air-gapped Systems [Master Article]](https://access.redhat.com/solutions/3176811)

- [Yum Repository Configuration Helper](https://access.redhat.com/labs/yumrepoconfighelper/)

  This tool will help you set up a simple Yum repository for your local machine or a small number of other machines to use. 
  
- [How to create local repository distributed through apache of Red Hat Enterprise Linux 5/6/7/8 using DVD iso for update or installation?](https://access.redhat.com/solutions/7227)


- [Configuring Virtual Machine Subscriptions in Red Hat Satellite](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.11/html-single/configuring_virtual_machine_subscriptions_in_red_hat_satellite/index)- RED HAT SATELLITE 6.11 - Using virt-who to manage host-based subscriptions
- [Simple Content Access](https://access.redhat.com/articles/simple-content-access)
- [Getting Started with Simple Content Access](https://access.redhat.com/documentation/en-us/subscription_central/2021/html/getting_started_with_simple_content_access/index)

- [Does Red Hat Satellite 6 and Red Hat Satellite Capsule 6 support hardening?](https://access.redhat.com/solutions/3198782)
## Useful Links

- [Red Hat on GitHub](https://redhatofficial.github.io/)


## Appendix

**How to fix Repos not Visible on Client issue ?**

goto https://access.redhat.com/labs/rhpc/

select the product -> version and arch
-> follow scripted or manual

