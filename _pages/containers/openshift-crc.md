---
layout: post
title: OpenShift Local (Formerly CodeReady Containers - CRC)
author: gini
categories: [ openshift, crc, openshift-local]
tags: [ OpenShift Installation, crc ]
show-avatar: false
permalink: openshift-local
featured: false
hidden: false
titleshort: openshift-local
---

- [Installing CodeReady Containers](#installing-codeready-containers)
  - [Download CRC Package](#download-crc-package)
  - [Required software packages](#required-software-packages)
  - [Enable sudo for user](#enable-sudo-for-user)
  - [Setup Cluster](#setup-cluster)
  - [Access your Cluster](#access-your-cluster)
  - [Enable cluster monitoring (metrics)](#enable-cluster-monitoring-metrics)
  - [Clean up](#clean-up)
- [Troubleshooting](#troubleshooting)
- [Installing CRC on GCP (WIP)](#installing-crc-on-gcp-wip)
  - [Create a GCP Instance with Nested Virtualization](#create-a-gcp-instance-with-nested-virtualization)
- [References:](#references)

[Red Hat CodeReady Containers](https://developers.redhat.com/products/codeready-containers)


## Installing CodeReady Containers

### Download CRC Package

Refer : **[Install OpenShift on a laptop with CodeReady Containers](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)**
Also check: [https://www.openshift.com/try](https://www.openshift.com/try)

- Visit [console.redhat.com/openshift/create/local](https://console.redhat.com/openshift/create/local)
- Choose install on Laptop -> https://cloud.redhat.com/openshift/install/crc/installer-provisioned
- Download for your OS choise (Windows10, MacOS, Linux)
- Move package to your machine folder
- extract the package 
  `tar -xf FILENAME.tar.xz`

- Download and keep the pull secret from same location. You need that later during `crc start`

### Required software packages

CodeReady Containers requires the libvirt and NetworkManager packages. 

```bash
yum install NetworkManager
wget https://mirror.openshift.com/pub/openshift-v4/clients/crc/latest/crc-linux-amd64.tar.xz
```

### Enable sudo for user

```
[devops@node1 ~]$ sudo cat /etc/sudoers.d/devops
[sudo] password for devops:
devops ALL=(ALL) NOPASSWD: ALL
```

### Setup Cluster

```bash
$ cd crc-linux-1.11.0-amd64
$ export PATH=/home/devops/crc-linux-1.11.0-amd64:$PATH

## set up your host operating system for the CodeReady Containers virtual machine.
## Use normal user account
$ crc setup

# Configure CRC
crc config set cpus 4
crc config set memory 
## Start cluster
## if you are on a terminal without GUI, you will have difficulty to copy/paste pull secret content. in that you can mention the pull secret ful
$ crc start -p /path-to/pull-secret
```

### Access your Cluster
```
$ eval $(crc oc-env)
$ oc login -u developer -p developer

## see credentials
$ crc console --credentials
To login as a regular user, run 'oc login -u developer -p developer https://api.crc.testing:6443'.
To login as an admin, run 'oc login -u kubeadmin -p 8rynV-SeYLc-h8Ij7-YPYcz https://api.crc.testing:6443'

## Access Console
$ crc console
Opening the OpenShift Web Console in the default browser...
```

### Enable cluster monitoring (metrics)

CodeReady Containers disables the machine-config and monitoring Operators by default.

To enable: 

`crc config set enable-cluster-monitoring true`

### Clean up

```shell
$ crc cleanup
```


## Troubleshooting

- **Issue** : After crc start and crc console, oc login fails with Internal error occurred: unexpected response: 503 for a while #740

- **Issue** : Unable to connect to console

```
$ crc ip
$ nmcli conn show
```

## Installing CRC on GCP (WIP)

### Create a GCP Instance with Nested Virtualization

```shell
# create the instance with nested virtualization
# add other parameters as needed
gcloud compute instances create VM_NAME \
  --enable-nested-virtualization \
  --zone=ZONE \
  --min-cpu-platform="Intel Haswell"
  
# login to VM
gcloud compute ssh VM_NAME

# check nested virtualization
grep -cw vmx /proc/cpuinfo
```


## References: 
- [CodeRead Containers - Red Hat OpenShift 4 on your laptop](https://developers.redhat.com/blog/2019/09/05/red-hat-openshift-4-on-your-laptop-introducing-red-hat-codeready-containers/)
- [Install OpenShift Container Platform 4](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)
- [CodeReady Containers on Ubuntu](https://labs.consol.de/devops/linux/2019/11/29/codeready-containers-on-ubuntu.html)
- [Trying out CRC - Jeff](https://www.jeffgeerling.com/blog/2019/trying-out-crc-code-ready-containers-run-openshift-4x-locally)
- [RED HAT CODEREADY CONTAINERS](https://access.redhat.com/documentation/en-us/red_hat_codeready_containers/1.13/html-single/getting_started_guide/index)
- [Accessing CodeReady Containers on a Remote Server](https://cloud.redhat.com/blog/accessing-codeready-containers-on-a-remote-server/)