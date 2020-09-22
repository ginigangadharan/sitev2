---
layout: post
title: Ansible Collections
categories: [ ansible ]
tags: [ansible]
show-avatar: false
permalink: ansible-collections
featured: false
hidden: false
showindex: true
---
<!-- TOC depthfrom:1 orderedlist:false -->

- [Installing collections](#installing-collections)
  - [Using Collection on Tower](#using-collection-on-tower)
- [References](#references)

<!-- /TOC -->


# Installing collections

```
ansible-galaxy collection install my_namespace.my_collection
```
or
```
$ cat collections/requirements.yml
collections:
  - name: junipernetworks.junos
    source: https://galaxy.ansible.com
 
  - name: f5networks.f5_modules
    source: https://cloud.redhat.com/api/automation-hub/
```

```
ansible-galaxy collection install -r collections/requirements.yml 
```

## Using Collection on Tower

[Installing and using collections on Ansible Tower](https://www.ansible.com/blog/installing-and-using-collections-on-ansible-tower)


# References

- **[Getting Started With Ansible Content Collections](https://www.ansible.com/blog/getting-started-with-ansible-collections)**
- **[Using collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html)**
- **[Hands on with Ansible collections](https://www.ansible.com/blog/hands-on-with-ansible-collections)**

