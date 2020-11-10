---
layout: post
title: Connecting Ansible Tower to Git Server with Self Signed Certificates
author: gini
categories: [ Ansible ]
image: "assets/images/2020/connecting-ansible-tower-to-git-server-with-self-signed-certificates-1024x683.png"
tags: [ ansible ]
ext_url: https://www.techbeatly.com/2020/10/connecting-ansible-tower-to-git-server-with-self-signed-certificates.html
#show-avatar: false
featured: false
hidden: false
---

So many questioned me when I mention git server in an Ansible Tower environment; and later I realized that, most of them are keeping their projects inside Ansible Tower !!!

Okay, that is just an option in Ansible Tower and Highly NOT Recommended for production setup or for environment with multiple teams accessing the Ansible Tower.

**Why Should Not ?**

- Creating a new Project in Ansible Tower
- You need to give permission for each and every user to this Ansible Tower node and to this directory – /var/lib/awx/projects
- It won’t be effective when you deploy Ansible Tower as multi-node cluster as you need to update the playbooks and project files in every node, under /var/lib/awx/projects
- Less control on editing playbooks or files as you have to do everything from CLI (mostly Ansible Tower nodes installs on nodes without GUI)
- Other users can easily access other projects and files if you didn’t configure your directory properly.
- and many other reasons…

Read Full Blog here -  **[Connecting Ansible Tower to Git Server with Self Signed Certificates](https://www.techbeatly.com/2020/10/connecting-ansible-tower-to-git-server-with-self-signed-certificates.html)**

- **[Subscribe to YouTube Channel](https://www.youtube.com/techbeatly?sub_confirmation=1)**
