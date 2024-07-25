---
layout: post
title: Ansible for Network Automation
categories: [ cloud ]
tags: []
show-avatar: false
permalink: ansible-for-network
featured: false
hidden: false
titleshort: Ansible Network
---

<!-- TOC orderedlist:false depthfrom:2 -->

- [References](#references)
- [Privilege Escalation for Network Devices in Ansible](#privilege-escalation-for-network-devices-in-ansible)
- [Communication Protocols](#communication-protocols)
- [Network modules](#network-modules)
- [Ansible Network Playbooks](#ansible-network-playbooks)
- [Ansible Network Roles](#ansible-network-roles)
- [Task Reference](#task-reference)
  - [Using username and password for authentication](#using-username-and-password-for-authentication)
  - [Reboot ios device](#reboot-ios-device)
  - [Backup eos](#backup-eos)
  - [Change config](#change-config)
  - [Add VLAN nxos](#add-vlan-nxos)
  - [Add ACL (Access Control List)](#add-acl-access-control-list)
  - [Add config iso\_config](#add-config-iso_config)
  - [Resources](#resources)
  - [Appendix](#appendix)
    - [Download IOS Images](#download-ios-images)
  - [Reference Repos](#reference-repos)

<!-- /TOC -->


Read : [How Network Automation is Different](https://docs.ansible.com/ansible/latest/network/getting_started/network_differences.html)

# References
- **[Ansible for Network Automation Tutorial](https://www.networkcomputing.com/networking/ansible-network-automation-tutorial)**
- **[Ansible for Network Automation](https://docs.ansible.com/ansible/latest/network/index.html)**
- **[Red Hat : NETWORK AUTOMATION WITH ANSIBLE](https://www.ansible.com/overview/networking)**
- **[Automating Network VLAN Deployments with Ansible](https://skyenet.tech/automating-network-vlan-deployments-with-ansible/)**
- **[Networking with Ansible 104](https://dodgydudes.se/ansible-net104/)**
- **[ANSIBLE - VLAN PROVISIONING](https://thenetworkstack.com/ansible-vlan-provisioning/)**


- [Using Ansible to Mitigate Network Vulnerabilities](https://www.ansible.com/blog/using-ansible-to-mitigate-network-vulnerabilities)


# Privilege Escalation for Network Devices in Ansible

Sample environment variable

```
ansible_connection: network_cli
ansible_network_os: ios
ansible_become: yes
ansible_become_method: enable
```

# Communication Protocols

| ansible_connection |	Protocol |	Requires |	Persistent? |
|--------------------|-----------|-----------|--------------|
| network_cli	| CLI over SSH        | network_os setting  | yes |
| netconf	    | XML over SSH        |	network_os setting  | yes |
| httpapi     |	API over HTTP/HTTPS	| network_os setting  | yes |
| local	      | depends on provider |	provider setting    |	no  |

# Network modules

Arista EOS = eos_*
Cisco IOS/IOS-XE = ios_*
Cisco NX-OS = nxos_*
Cisco IOS-XR = iosxr_*
F5 BIG-IP = bigip_*
F5 BIG-IQ = bigiq_*
Juniper Junos = junos_*
VyOS = vyos_*

and modules as
- *_facts
- *_command
- *_config

And more

# Ansible Network Playbooks

Sample Playbookf or `ios`

```
- name: configure cisco routers
  hosts: routers
  connection: network_cli
  gather_facts: no
  vars:
    dns: "8.8.8.8 8.8.4.4"

  tasks:
   - name: configure hostname
     ios_config:
       lines: hostname {{ inventory_hostname }}

   - name: configure DNS
     ios_config:
       lines: ip name-server {{dns}}
```

Another one for interface config
![Ansible Playbook](assets/images/2020/ansible-network-ios-interface-playbook.png)

# Ansible Network Roles

https://galaxy.ansible.com/ansible-network

**network-engine**
This role provides the foundation for building network roles by providing modules and plugins that are common to all Ansible Network roles.

[Galaxy](https://galaxy.ansible.com/ansible-network/network-engine)

**config_manager**

This role is designed to provide a network platform agnostic approach to managing the active (running) configuration file on a remote device. This role requires one (or more) platform provider roles to execute properly.
[Galaxy](https://galaxy.ansible.com/ansible-network/config_manager)

Install roles

```
ansible-galaxy install ansible-network.cisco_ios
ansible-galaxy install ansible-network.config_manager
```

Update existing role

```
ansible-galaxy install ansible-network.network_engine,v2.7.0 --force
```

# Task Reference

## Using username and password for authentication

```
- name: User usernname
  vars:
    cli:
      username: user1
      password: password
      transport: cli
  tasks:
    - name: Test Login
      ios_config:
        provider: "{{ cli }}
        .
        .
```

## Reboot ios device

```
---
- name: reboot ios device
  cli_command:
    command: reload
    prompt:
      - Save?
      - confirm
    answer:
     - y
     - y

  # To make sure the current connection to the network device
  # is closed so that the socket can be reestablished to the network
  # device after the reboot takes place.
- name: reset the connection
  meta: reset_connection


- name: Wait for the network device to reload
  wait_for_connection:
    delay: 10
```

## Backup eos

Backup configuration

```
---
- name: BACKUP NETWORK CONFIGURATIONS
  hosts: arista
  gather_facts: false

  tasks:

    - name: BACKUP CONFIG
      eos_config:
        backup: yes
```

Backup using `cli_command`

- run arbitrary commands on network devices using `cli_command`

```
---
- name: RUN COMMAND AND PRINT TO TERMINAL WINDOW
  hosts: arista
  gather_facts: false

  tasks:

    - name: RUN ARISTA COMMAND
      cli_command:
        command: show run
      register: backup

    - name: PRINT TO TERMINAL WINDOW
      copy:
        content: "{{backup.stdout}}"
        dest: "{{inventory_hostname}}.backup"
```

## Change config

```
# vars
show_interfaces: "show ip interface brief"
backup: "show running-config"
save: "write memory"
ntp_commands: ntp server 192.168.1.1

---
- name: CHANGE CONFIGURATION
  hosts: routers
  gather_facts: false

  tasks:

    - name: LOAD NTP CONFIGURATION
      cli_config:
        config: "{{ntp_commands}}"
      notify:
        - SAVE CONFIGURATION

  handlers:

    - name: SAVE CONFIGURATION
      cli_command:
        command: "{{save}}"

# Show interface
    - name: RUN SHOW COMMAND
      cli_command:
        command: "{{show_interfaces}}"
      register: command_output
```

## Add VLAN nxos

```
---
- name: deploy vlans
  hosts: cisco
  gather_facts: no

  tasks:
    - name: ensure vlans exist
      nxos_vlan:
        vlan_id: 100
        admin_state: up
        name: WEB
```

## Add ACL (Access Control List)

https://dodgydudes.se/ansible-net104/


## Add config iso_config

```
---
- name: snmp ro/rw string configuration
  hosts: cisco
  gather_facts: no
  tasks:
    - name: ensure snmp strings are present
      ios_config:
        lines:
          - snmp-server community ansible-public RO
          - snmp-server community ansible-private RW
```

## Resources

- [Network Debug and Troubleshooting Guide](https://docs.ansible.com/ansible/latest/network/user_guide/network_debug_troubleshooting.html)


## Appendix

- DEVNET developer.cisco.com -> https://developer.cisco.com/site/sandbox/ -> https://devnetsandbox.cisco.com/

eg:  IOS XE on CSR Latest Code Always On
https://devnetsandbox.cisco.com/RM/Diagram/Index/38ded1f0-16ce-43f2-8df5-43a40ebf752e?diagramType=Topology

add variables

[routers:vars]
ansile_user=developer
ansible_password=password
ansible_connection=network_cli
ansible_network_os=iso
ansible_port=8181 #if diff port

### Download IOS Images
- [Where do I get IOS images?](https://docs.gns3.com/1vJwh4_whwtfjb8pQ8vKekcWrA1galIqA1eHgeClOsPY/index.html)
- Virl.cisco.com

## Reference Repos

- https://github.com/nleiva/ansible-networking
- https://github.com/zjleblanc/ansible-network-mgmt/tree/master
- https://github.com/zjleblanc/ansible-cisco-demos