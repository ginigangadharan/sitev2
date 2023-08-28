---
layout: post
title: Ansible Automation Platform - Frequently Asked Questions
# 1. author: gini
categories: [ cloud ]
# 2. image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: [cloud, automation, containers, kubernetes]
show-avatar: false
permalink: aap-faq
featured: false
hidden: false
titleshort: AWX
---

- [AAP on OpenShift](#aap-on-openshift)
  - [What are user\_id will be created in each server/container . (Ex- In controller , hub , db and execution node )](#what-are-user_id-will-be-created-in-each-servercontainer--ex--in-controller--hub--db-and-execution-node-)
  - [What are file system and directory will be created/needed in each AAP component(server/container).](#what-are-file-system-and-directory-will-be-createdneeded-in-each-aap-componentservercontainer)
  - [Namespace requirement –Ie AAP need to be visible to all namespace or separate one only .](#namespace-requirement-ie-aap-need-to-be-visible-to-all-namespace-or-separate-one-only-)
  - [Number of persistent storage for each AAP component ( EX- controller , db ) with small summary of what information will be save on such storage .)](#number-of-persistent-storage-for-each-aap-component--ex--controller--db--with-small-summary-of-what-information-will-be-save-on-such-storage-)


## AAP on OpenShift

### What are user_id will be created in each server/container . (Ex- In controller , hub , db and execution node )

The AAP operator, controller, hub, execution and database pods are running using the official Red Hat container images. Most of them contains the `awx` user and other user account. There is no ready-to-share list of such information but you can find the following details under the container image page.

- Overview
- Security
- Technical Information
- Packages
- Dockerfile
- Get this image

Example URL for [Ansible Automation Platform controller(
ansible-automation-platform-23/controller-rhel8)](
https://catalog.redhat.com/software/containers/search?q=ansible-automation-platform-23&p=1) container image.

###  What are file system and directory will be created/needed in each AAP component(server/container).

These are container images and the files systems are pre-defined and no additional configurations is required. For the pods which required `persistentvolumes` (controller, hub, database etc), the required `persistentvolumeclaims` will be created by the AAP operator. This is customizable and users can define the size during the deployment. (Part of information gathering)

### Namespace requirement –Ie AAP need to be visible to all namespace or separate one only .

AAP Operator is can be installed in all namespaces but recommended to keep it in its own namespace. 

### Number of persistent storage for each AAP component ( EX- controller , db ) with small summary of what information will be save on such storage .)

See the answer above.

 