---
layout: post
title: Ansible - Questions and References
author: gini
categories: [ Ansible, kubernetes ]
tags: [ OpenShift, kubernetes ]
image: "assets/images/2020/how-to-create-increase-or-decrease-project-quota-in-openshift.jpg"
show-avatar: false
permalink: ansible-questions
featured: false
hidden: false
titleshort: Ansible Questions
---

*Note: These are questions I have received via chat groups and communities. This is a living document and I will update the page whenever there is a new question or better answer or references.*

- [What is Ansible](#what-is-ansible)
- [What is Ansible Tower](#what-is-ansible-tower)
- [Why Ansible is different than other automation tools](#why-ansible-is-different-than-other-automation-tools)
- [Can I use Ansible for Auto Remediation ?](#can-i-use-ansible-for-auto-remediation-)
- [What is Instance Groups and Isolated Nodes in Ansibe Tower ?](#what-is-instance-groups-and-isolated-nodes-in-ansibe-tower-)


## What is Ansible  

**Ansible** is an open source automation tool with simple automation language (YAML)

## What is Ansible Tower

Ansible Tower is an enterprise framework for controlling, securing and managing your Ansible Automation with UI and RESTful API

## Why Ansible is different than other automation tools

There are other automation tools like Puppet, Chef, Saltstack etc but they all need a agent to be run on managed nodes. Ansible is agent-less; as long as Ansible node have access to managed node via SSH, API etc, Ansible can manage that node.

[Reference](https://www.whizlabs.com/blog/chef-vs-puppet-vs-ansible/)

## Can I use Ansible for Auto Remediation ?

Yes, you can use Ansible for auto-remediation; but keep in mind that someone has to trigger the Ansible job either manually or via some integrated example. 

Eg: 

- Step 1: Your monitoring software (nagios, icinga, zabbix, Prometheus etc) detects an issue on system configuration (like unauthorized sudo user, or low disk space).
- Step 2: Monitoring tool will create a ticket (ServiceNow, Jira, Slack etc)
- Step 3: Monitoring tool will trigger a call to Ansible Job (custmized ) to remediate.
- Step 4: Ansible will run the job on target node and return result to Monitoring/Ticketing Tool.

## What is Instance Groups and Isolated Nodes in Ansibe Tower ?

An Ansible Tower **Instance group** is a set of cluster nodes dedicated for a particular purpose. You can organize your Ansible Tower Cluster into any number of instance groups, and cluster nodes can exist in multiple instance groups. Each instance group has its own job queue, and any node in the group can take jobs off of that queue.

An **Isolated Node** is an Ansible Tower node that contains a small piece of software for running playbooks locally to manage a set of infrastructure. It can be deployed behind a firewall/VPC or in a remote datacenter, with only SSH access available. 

- [Instance Groups and Isolated Nodes - Ansible Blog](https://www.ansible.com/blog/ansible-tower-feature-spotlight-instance-groups-and-isolated-nodes)