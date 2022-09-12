---
layout: post
title: Ansible Dictionary Sample
categories: [ ansible ]
tags: [ansible]
show-avatar: false
permalink: ansible-dictionary
featured: false
hidden: false
showindex: false
---

Sample Playbook to Handle Ansible dictionary

```
---
- name: Test play for dictionary
  hosts: all
  vars:
    interfaces:
      ansible-node-2:
        int1:
          intname: ens192
          intip: 10.154.158.146
          intmask: 255.255.255.128
          intgw: 10.154.158.1
  tasks:
    - name: Looping
      debug:
        msg: Hello {{ mydict }}
      #with_dict:
      #  - '{{ ens192 }}'

    - name: Looping 2
      debug:
        msg: "Int1 : {{ interfaces['ansible-node-2']['int1']['intname'] }}"
```