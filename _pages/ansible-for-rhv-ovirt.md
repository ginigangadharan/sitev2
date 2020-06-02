---
layout: post
title: Ansible and oVirt/Red Hat Virtualization
categories: [ cloud ]
#image: "assets/images/2020/ansible-banner-logo-transparent.jpg"
tags: [cloud, automation, ovirt]
permalink: /ansible-ovirt-rhv
featured: false
hidden: false
---

## Modules
- rhevm - https://docs.ansible.com/ansible/latest/modules/rhevm_module.html
    - supports oVirt/RHEV version 3.
- ovirt_vm - https://docs.ansible.com/ansible/latest/modules/ovirt_vm_module.html
    - need `ovirt-engine-sdk-python` >= 4.3.0
```
sudo apt install python-pip
apt-get install python-lxml
apt-cache depends python-pycurl
# then install all dependancies
sudo apt-get install libcurl4-gnutls-dev
sudo apt install libcurl4-gnutls-dev librtmp-dev
pip install ovirt-engine-sdk-python
```
Ref: https://github.com/oVirt/ovirt-engine-sdk/issues/18

https://calgaryrhce.ca/blog/2018/10/23/using-ansible-to-manage-rhv-ovirt/


## rhevm
```
- name: Basic get info from VM
  rhevm:
    server: rhevm01
    user: '{{ rhev.admin.name }}'
    password: '{{ rhev.admin.pass }}'
    name: demo
    state: info

- name: Basic create example from image
  rhevm:
    server: rhevm01
    user: '{{ rhev.admin.name }}'
    password: '{{ rhev.admin.pass }}'
    name: demo
    cluster: centos
    image: centos7_x64
    state: present

- name: Power management
  rhevm:
    server: rhevm01
    user: '{{ rhev.admin.name }}'
    password: '{{ rhev.admin.pass }}'
    cluster: RH
    name: uptime_server
    image: centos7_x64
    state: down

```    

## ovirt_vm
- use [`ovirt_auth module`](https://docs.ansible.com/ansible/2.6/modules/ovirt_auth_module.html#ovirt-auth-module) to reuse authentication

```
- name: Creates a new Virtual Machine from template named 'rhel7_template'
  ovirt_vm:
    state: present
    name: myvm
    template: rhel7_template
    cluster: mycluster

- name: Register VM
  ovirt_vm:
    state: registered
    storage_domain: mystorage
    cluster: mycluster
    name: myvm
```

## All modules
- ovirt - oVirt/RHEV platform management
- ovirt_affinity_group - Module to manage affinity groups in oVirt/RHV
- ovirt_affinity_label - Module to manage affinity labels in oVirt/RHV
- ovirt_affinity_label_facts - Retrieve facts about one or more oVirt/- RHV affinity labels
- ovirt_api_facts - Retrieve facts about the oVirt/RHV API
- ovirt_auth - Module to manage authentication to oVirt/RHV
- ovirt_cluster - Module to manage clusters in oVirt/RHV
- ovirt_cluster_facts - Retrieve facts about one or more oVirt/RHV - clusters
- ovirt_datacenter - Module to manage data centers in oVirt/RHV
- ovirt_datacenter_facts - Retrieve facts about one or more oVirt/RHV - datacenters
- ovirt_disk - Module to manage Virtual Machine and floating disks in - oVirt/RHV
- ovirt_disk_facts - Retrieve facts about one or more oVirt/RHV disks
- ovirt_external_provider - Module to manage external providers in oVirt/- RHV
- ovirt_external_provider_facts - Retrieve facts about one or more oVirt/- RHV external providers
- ovirt_group - Module to manage groups in oVirt/RHV
- ovirt_group_facts - Retrieve facts about one or more oVirt/RHV groups
- ovirt_host_networks - Module to manage host networks in oVirt/RHV
- ovirt_host_pm - Module to manage power management of hosts in oVirt/RHV
- ovirt_host_storage_facts - Retrieve facts about one or more oVirt/RHV - HostStorages (applicable only for block storage)
- ovirt_hosts - Module to manage hosts in oVirt/RHV
- ovirt_hosts_facts - Retrieve facts about one or more oVirt/RHV hosts
- ovirt_mac_pools - Module to manage MAC pools in oVirt/RHV
- ovirt_networks - Module to manage logical networks in oVirt/RHV
- ovirt_networks_facts - Retrieve facts about one or more oVirt/RHV - networks
- ovirt_nics - Module to manage network interfaces of Virtual Machines - in oVirt/RHV
- ovirt_nics_facts - Retrieve facts about one or more oVirt/RHV virtual - machine network interfaces
- ovirt_permissions - Module to manage permissions of users/groups in - oVirt/RHV
- ovirt_permissions_facts - Retrieve facts about one or more oVirt/RHV - permissions
- ovirt_quotas - Module to manage datacenter quotas in oVirt/RHV
- ovirt_quotas_facts - Retrieve facts about one or more oVirt/RHV quotas
- ovirt_scheduling_policies_facts - Retrieve facts about one or more - oVirt scheduling policies
- ovirt_snapshots - Module to manage Virtual Machine Snapshots in oVirt/- RHV
- ovirt_snapshots_facts - Retrieve facts about one or more oVirt/RHV - virtual machine snapshots
- ovirt_storage_connections - Module to manage storage connections in - oVirt
- ovirt_storage_domains - Module to manage storage domains in oVirt/RHV
- ovirt_storage_domains_facts - Retrieve facts about one or more oVirt/- RHV storage domains
- ovirt_storage_templates_facts - Retrieve facts about one or more oVirt/- RHV templates relate to a storage domain.
- ovirt_storage_vms_facts - Retrieve facts about one or more oVirt/RHV - virtual machines relate to a storage domain.
- ovirt_tags - Module to manage tags in oVirt/RHV
- ovirt_tags_facts - Retrieve facts about one or more oVirt/RHV tags
- ovirt_templates - Module to manage virtual machine templates in oVirt/- RHV
- ovirt_templates_facts - Retrieve facts about one or more oVirt/RHV - templates
- ovirt_users - Module to manage users in oVirt/RHV
- ovirt_users_facts - Retrieve facts about one or more oVirt/RHV users
- ovirt_vmpools - Module to manage VM pools in oVirt/RHV
- ovirt_vmpools_facts - Retrieve facts about one or more oVirt/RHV - vmpools
- ovirt_vms - Module to manage Virtual Machines in oVirt/RHV
- ovirt_vms_facts - Retrieve facts about one or more oVirt/RHV virtual machines