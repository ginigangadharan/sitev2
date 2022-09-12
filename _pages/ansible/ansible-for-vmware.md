---
layout: post
title: Ansible for VMware
categories: [ cloud ]
tags: []
show-avatar: false
permalink: ansible-for-vmware
featured: false
hidden: false
titleshort: Ansible VMWare
---


Ref: [VMware Guide for Ansible](https://docs.ansible.com/ansible/latest/scenario_guides/guide_vmware.html)

- [1. VMware Prerequisites](#1-vmware-prerequisites)
  - [1.1. vmware_guest module](#11-vmware_guest-module)
    - [1.1.1. Installing vCenter SSL certificates for Ansible](#111-installing-vcenter-ssl-certificates-for-ansible)
    - [1.1.2. Installing ESXi SSL certificates for Ansible](#112-installing-esxi-ssl-certificates-for-ansible)
- [2. Appendix](#2-appendix)
  - [2.1. VMWware Templates](#21-vmwware-templates)
  - [2.2. Enable custom scripts in vmware-tools](#22-enable-custom-scripts-in-vmware-tools)
  - [2.3. vCenter Simumlator Container](#23-vcenter-simumlator-container)
  - [2.4. Useful Links](#24-useful-links)
# 1. VMware Prerequisites

Ansible VMware modules are written on top of pyVmomi which is the Python SDK for the VMware vSphere API that allows user to manage ESX, ESXi, and vCenter infrastructure. 

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py

```
$ pip install pyvmomi
```

## 1.1. vmware_guest module
The vmware_guest module manages various operations related to virtual machines in the given ESXi or vCenter server.

### 1.1.1. Installing vCenter SSL certificates for Ansible
From any web browser, go to the base URL of the vCenter Server without port number like https://vcenter-domain.example.com
Click the “Download trusted root CA certificates” link at the bottom of the grey box on the right and download the file.
Change the extension of the file to .zip. The file is a ZIP file of all root certificates and all CRLs.
Extract the contents of the zip file. The extracted directory contains a .certs directory that contains two types of files. Files with a number as the extension (.0, .1, and so on) are root certificates.
Install the certificate files are trusted certificates by the process that is appropriate for your operating system.

### 1.1.2. Installing ESXi SSL certificates for Ansible
Enable SSH Service on ESXi either by using Ansible VMware module vmware_host_service_manager or manually using vSphere Web interface.
SSH to ESXi server using administrative credentials, and navigate to directory /etc/vmware/ssl
Secure copy (SCP) rui.crt located in /etc/vmware/ssl directory to Ansible control node.
Install the certificate file by the process that is appropriate for your operating system.

# 2. Appendix

## 2.1. VMWware Templates

https://www.nakivo.com/blog/vm-templates-a-to-z/

## 2.2. Enable custom scripts in vmware-tools
vmware-toolbox-cmd config set deployPkg enable-custom-scripts true

Ref: [Create a Customization Specification for Linux](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.vm_admin.doc/GUID-9A5093A5-C54F-4502-941B-3F9C0F573A39.html)

## 2.3. vCenter Simumlator Container
- [vCenter Simulator Docker Container](https://brianbunke.com/blog/2018/12/31/vcenter-simulator-ci/)
- [vCenter and ESi API based simulator](https://hub.docker.com/r/nimmis/vcsim)
- [ansible / vcenter-test-container](https://quay.io/repository/ansible/vcenter-test-container)
- [vcenter-test-container](https://github.com/ansible/vcenter-test-container)

## 2.4. Useful Links
- [Enum - VirtualMachineGuestOsIdentifier](https://www.vmware.com/support/developer/converter-sdk/conv55_apireference/vim.vm.GuestOsDescriptor.GuestOsIdentifier.html)