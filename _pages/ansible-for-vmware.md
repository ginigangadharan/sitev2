---
layout: post
title: Ansible for VMware
categories: [ cloud ]
tags: []
show-avatar: false
permalink: ansible-for-vmware
featured: false
hidden: false
---

Ref: [VMware Guide for Ansible](https://docs.ansible.com/ansible/latest/scenario_guides/guide_vmware.html)

<!-- TOC -->

- [.1. Requirements](#1-requirements)
- [.2. vmwareguest module](#2-vmwareguest-module)
- [.3. VMware Prerequisites](#3-vmware-prerequisites)
  - [.3.1. Installing vCenter SSL certificates for Ansible](#31-installing-vcenter-ssl-certificates-for-ansible)
  - [.3.2. Installing ESXi SSL certificates for Ansible](#32-installing-esxi-ssl-certificates-for-ansible)
- [1.1. VMWware Templates](#11-vmwware-templates)

<!-- /TOC -->

## .1. Requirements
Ansible VMware modules are written on top of pyVmomi which is the Python SDK for the VMware vSphere API that allows user to manage ESX, ESXi, and vCenter infrastructure. 

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py

```
$ pip install pyvmomi
```

## .2. vmware_guest module
The vmware_guest module manages various operations related to virtual machines in the given ESXi or vCenter server.

## .3. VMware Prerequisites

### .3.1. Installing vCenter SSL certificates for Ansible
From any web browser, go to the base URL of the vCenter Server without port number like https://vcenter-domain.example.com
Click the “Download trusted root CA certificates” link at the bottom of the grey box on the right and download the file.
Change the extension of the file to .zip. The file is a ZIP file of all root certificates and all CRLs.
Extract the contents of the zip file. The extracted directory contains a .certs directory that contains two types of files. Files with a number as the extension (.0, .1, and so on) are root certificates.
Install the certificate files are trusted certificates by the process that is appropriate for your operating system.

### .3.2. Installing ESXi SSL certificates for Ansible
Enable SSH Service on ESXi either by using Ansible VMware module vmware_host_service_manager or manually using vSphere Web interface.
SSH to ESXi server using administrative credentials, and navigate to directory /etc/vmware/ssl
Secure copy (SCP) rui.crt located in /etc/vmware/ssl directory to Ansible control node.
Install the certificate file by the process that is appropriate for your operating system.

# 1. Appendix

## 1.1. VMWware Templates

https://www.nakivo.com/blog/vm-templates-a-to-z/