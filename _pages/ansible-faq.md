---
layout: post
title: Ansible - Frequently Asked Questions
categories: [ ansible ]
tags: [ansible]
show-avatar: false
permalink: ansible-faq
featured: false
hidden: false
showindex: true
---
<!-- TOC depthfrom:3 orderedlist:false -->

- [What is Ansible](#what-is-ansible)
- [What is Ansible Tower](#what-is-ansible-tower)
- [Why Ansible is different than other automation tools](#why-ansible-is-different-than-other-automation-tools)
- [Can I use Ansible for Auto Remediation ?](#can-i-use-ansible-for-auto-remediation)

<!-- /TOC -->

### What is Ansible  

**Ansible** is an open source automation tool with simple automation language (YAML)

### What is Ansible Tower

Ansible Tower is an enterprise framework for controlling, securing and managing your Ansible Automation with UI and RESTful API

### Why Ansible is different than other automation tools

There are other automation tools like Puppet, Chef, Saltstack etc but they all need a agent to be run on managed nodes. Ansible is agent-less; as long as Ansible node have access to managed node via SSH, API etc, Ansible can manage that node.

[Reference](https://www.whizlabs.com/blog/chef-vs-puppet-vs-ansible/)

### Can I use Ansible for Auto Remediation ?

Yes, you can use Ansible for auto-remediation; but keep in mind that someone has to trigger the Ansible job either manually or via some integrated example. 

Eg: 

- Step 1: Your monitoring software (nagios, icinga, zabbix, Prometheus etc) detects an issue on system configuration (like unauthorized sudo user, or low disk space).
- Step 2: Monitoring tool will create a ticket (ServiceNow, Jira, Slack etc)
- Step 3: Monitoring tool will trigger a call to Ansible Job (custmized ) to remediate.
- Step 4: Ansible will run the job on target node and return result to Monitoring/Ticketing Tool.



If Incident remediation is caused by user or hacker which resulted in a change in configurations, will the roll-back be automated? Or will user intervention be required?
from WinHninThida to everyone:
yes. Ansible is widely used for config managment.
from WinHninThida to everyone:
we can easily roll back to the previous config if hacker or someone did incidentally.
from Shi Lun Lences Ang to everyone:
Noted. So the roll-back need no user intervention. Great
from WinHninThida to everyone:
without any manual intervention. Of course, someone need to login to Ansible tower and apply the roll back.
from Shi Lun Lences Ang to everyone:
For Ansible, can Client use the playbooks to run and re-use their existing scripts, written in Python or Ruby for example?
from WinHninThida to everyone:
Yaml. it's the simple human readable plain text.
from WinHninThida to everyone:
No programing language is required.
from Shi Lun Lences Ang to everyone:
Which means they cannot re-use their existing scripts?
from WinHninThida to everyone:
Ansible can call the existing scripts or you can convert the script to yaml. 
from WinHninThida to everyone:
if the script is super good and power. Usually, we are not recommended to replace. 
from WinHninThida to everyone:
Lences, you can feel free ask to the group. so that everyone can hear.
from Shi Lun Lences Ang to everyone:
:D okie dokie
from WinHninThida to everyone:
:)
from SurashK to everyone:
wah you all understand? 
from Shi Lun Lences Ang to everyone:
Ling Ling did a good job explaining :)
from SurashK to everyone:
wah then any questions can ask Lences :)