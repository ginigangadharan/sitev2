---
layout: post
title: Ansible wait for methods
categories: [ Ansible ]
tags: [automation]
show-avatar: false
permalink: ansible-wait-for-samples
featured: false
hidden: false
showindex: true
---

Ansible sample methods to wait for a machine to boot or a port to be ready.

```yaml
    - name: Waits for SSH, don't start checking for 5 seconds
      wait_for:
        host: "{{ inventory_hostname }}"
        port: 22
        delay: 10
        timeout: 300
        state: started

    - name: Wait for VMs to boot up (300, 5)
      wait_for_connection:
        delay: 5
        timeout: 300
        sleep: 5

    - name: check SSH access
      local_action: shell ansible -u {{ ansible_user_id }} -m ping {{ inventory_hostname }}
      register: result
      until: result.rc == 0
      retries: 30
      delay: 10

    - name: Wait for VMs to boot with SSH
      local_action:
        module: wait_for host={{ inventory_hostname }} port=22 delay=10 connect_timeout=5 timeout=300
```        