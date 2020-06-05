---
layout: post
title: OpenShift Installation Methods - Examples
categories: [ openshift ]
tags: [ OpenShift Installation ]
show-avatar: false
permalink: /openshift-installation
featured: false
hidden: false
---

<!-- TOC orderedlist:true -->

- [1. Method I - Setup an OpenShift All-In-One](#1-method-i---setup-an-openshift-all-in-one)
  - [1.1. Install required packages](#11-install-required-packages)
  - [1.2. Disable Firewall](#12-disable-firewall)
  - [1.3. Installing OpenShift CLI](#13-installing-openshift-cli)
    - [1.3.1. Method 1 - Standard CentOS repositories](#131-method-1---standard-centos-repositories)
    - [1.3.2. Method 2 - Download and extract openshift origin](#132-method-2---download-and-extract-openshift-origin)
      - [1.3.2.1. Add the directory you untarred the release into to your path:](#1321-add-the-directory-you-untarred-the-release-into-to-your-path)
  - [1.4. Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16](#14-configure-the-docker-daemon-with-an-insecure-registry-parameter-of-172300016)
    - [1.4.1. Restart docker service](#141-restart-docker-service)
  - [1.5. Initiate cluster](#15-initiate-cluster)
    - [1.5.1. Ref:](#151-ref)
    - [1.5.2. Add a user](#152-add-a-user)
- [2. Method II - Setup minishift](#2-method-ii---setup-minishift)
  - [2.1. Setup Virtual Environment](#21-setup-virtual-environment)
  - [2.2. Install minishift](#22-install-minishift)
  - [2.3. Start minishift cluster](#23-start-minishift-cluster)
- [3. Method III - OpenShift 4 - All in One Quick Cluster](#3-method-iii---openshift-4---all-in-one-quick-cluster)
- [4. Method IV - OpenShift Full Cluster](#4-method-iv---openshift-full-cluster)
- [5. CodeRead Containers](#5-coderead-containers)
  - [5.1. Download Package](#51-download-package)
  - [5.2. Setup Cluster](#52-setup-cluster)
  - [5.3. Troubleshooing VirtualBox VM](#53-troubleshooing-virtualbox-vm)
  - [5.4. Access your Cluster](#54-access-your-cluster)
- [6. OpenShift on Baremetal (UPI)](#6-openshift-on-baremetal-upi)
- [7. OpenShift All-In-One - OKD Using Ansible](#7-openshift-all-in-one---okd-using-ansible)
- [8. Extras OpenSHift 4.x](#8-extras-openshift-4x)
  - [8.1. OpenShift 4.2 Installation](#81-openshift-42-installation)
  - [8.2. OpenShift 4.1 Installation](#82-openshift-41-installation)
  - [8.3. Baremetal Installation](#83-baremetal-installation)
- [9. baremetal](#9-baremetal)

<!-- /TOC -->

# 1. Method I - Setup an OpenShift All-In-One 

## 1.1. Install required packages
```
yum install ansible docker wget -y
systemctl enable docker
systemctl start docker
```

## 1.2. Disable Firewall
Or you have to open required ports.
```
systemctl disable firewalld
systemctl stop firewalld
```

## 1.3. Installing OpenShift CLI

### 1.3.1. Method 1 - Standard CentOS repositories
```
yum -y install centos-release-openshift-origin39
yum -y install origin-clients
```

### 1.3.2. Method 2 - Download and extract openshift origin

Create a directory for data (anywhere)
```
mkdir /data
cd /data
```

Check the latest version if you need.
```
wget "https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz"
tar -xzvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz 
cd openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit
```

#### 1.3.2.1. Add the directory you untarred the release into to your path:
```
export PATH=$PATH:/data/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit/

# or

export PATH=$PATH:`pwd`
```

## 1.4. Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16
```
cat > /etc/docker/daemon.json <<DELIM
{
   "insecure-registries": [
     "172.30.0.0/16"
   ]
}
DELIM
```

### 1.4.1. Restart docker service
```
service docker restart
```

## 1.5. Initiate cluster
```
oc cluster up --base-dir="/data/clusterup" --public-hostname=<IP>
```
- `--base-dir=BASE_DIR` : Directory on Docker host for cluster up configuration

(oc cluster up --public-hostname=35.239.51.76 --routing-suffix=35.239.51.76.xip.io)
openshift.local.clusterup/openshift-controller-manager/openshift-master.kubeconfig


### 1.5.1. Ref:
- https://github.com/openshift/origin/blob/release-3.11/docs/cluster_up_down.md
- https://medium.com/@fabiojose/working-with-oc-cluster-up-a052339ea219


### 1.5.2. Add a user
```
# oc create user redhat
user.user.openshift.io/redhat created
# oc adm policy add-cluster-role-to-user cluster-admin redhat
cluster role "cluster-admin" added: "redhat"
```

# 2. Method II - Setup minishift

## 2.1. Setup Virtual Environment

- setup KVM or virtualbox (or other virtulization)
Ref: [Set up your virtualization environment](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html)

## 2.2. Install minishift
- [Download](https://github.com/minishift/minishift/releases) and manually install 

or 

Installation on mac
```
brew cask install minishift
```

* in case issue to install `export HOMEBREW_NO_ENV_FILTERING=1`*

## 2.3. Start minishift cluster

```
minishift start --vm-driver virtualbox

# setup VirtualBox Permanently
minishift config set vm-driver virtualbox
```

Once started, access the console using url or open in browser
```
minishift console
```

setup oc access
```
$ minishift oc-env
export PATH="/home/john/.minishift/cache/oc/v1.5.0:$PATH"
# Run this command to configure your shell:
# eval $(minishift oc-env)
```

# 3. Method III - OpenShift 4 - All in One Quick Cluster

https://github.com/openshift/okd/releases

# 4. Method IV - OpenShift Full Cluster

- [Installing OpenShift 4.1 Using Libvirt and KVM](http://alesnosek.com/blog/2019/07/08/installing-openshift-4-dot-1-using-libvirt-and-kvm/)

# 5. CodeRead Containers

[Red Hat CodeReady Containers](https://developers.redhat.com/products/codeready-containers)
  
Ref: 
- [CodeRead Containers - Red Hat OpenShift 4 on your laptop](https://developers.redhat.com/blog/2019/09/05/red-hat-openshift-4-on-your-laptop-introducing-red-hat-codeready-containers/)
- [Install OpenShift Container Platform 4](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)
- [CodeReady Containers on Ubuntu](https://labs.consol.de/devops/linux/2019/11/29/codeready-containers-on-ubuntu.html)



## 5.1. Download Package
https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC

- Visit https://www.openshift.com/try
- Choose install on Laptop -> https://cloud.redhat.com/openshift/install/crc/installer-provisioned
- Download for your OS choise (Windows10, MacOS, Linux)
- Move package to your machine folder

## 5.2. Setup Cluster

```
### Usr normal user account
$ crc setup

### Start cluster
$ crc start
```

## 5.3. Troubleshooing VirtualBox VM

if issue with VirtualBox Guest Additions and shared folder
`sudo yum update && sudo yum -y install kernel-headers kernel-devel`

Check vboxsf module 
`sudo modprobe vboxsf`



## 5.4. Access your Cluster
```
$ eval $(crc oc-env)
$ oc login -u developer -p developer
```

# 6. OpenShift on Baremetal (UPI)

- [Installing on OpenShift on Baremetal](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.4/html/installing_on_bare_metal/index)

# 7. OpenShift All-In-One - OKD Using Ansible

Ref:
https://github.com/Gepardec/ansible-role-okd
https://galaxy.ansible.com/gepardec/okd
https://computingforgeeks.com/setup-openshift-origin-local-cluster-on-centos/

# 8. Extras OpenSHift 4.x 

https://github.com/openshift/okd

Create `clouds.yaml`
```
clouds:
  ocp4-dev:
    auth:
      auth_url: http://10.6.1.209:35357/
      project_name: ocp4-dev
      username: ocpadmin
      password: ocpadmin
    region_name: RegionOne
``` 

## 8.1. OpenShift 4.2 Installation
https://docs.openshift.com/container-platform/4.2/installing/installing_bare_metal/installing-bare-metal.html

## 8.2. OpenShift 4.1 Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/index

## 8.3. Baremetal Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/installing-on-bare-metal
https://docs.openshift.com/container-platform/4.1/installing/installing_bare_metal/installing-bare-metal.html
https://blog.openshift.com/openshift-4-bare-metal-install-quickstart/

https://blog.openshift.com/revamped-openshift-all-in-one-aio-for-labs-and-fun/

VMware
https://labs.consol.de/container/platform/openshift/2020/01/31/ocp43-installation-vmware.html

https://blogs.ovirt.org/2019/01/ovirt-openshift-part-1/

# 9. baremetal
https://docs.openshift.com/container-platform/4.3/installing/installing_bare_metal/installing-bare-metal.html#cluster-entitlements_installing-bare-metal