---
layout: post
title: Ansible Cheatsheet
author: gini
categories: [ cheatsheets ]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: cheatsheet-ansible
featured: false
hidden: false
showindex: true
titleshort: ansible cheatsheet
---

# Ansible Cheat Sheet

[Ansible](hhttps://www.techbeatly.com/ansible/) is an open-source IT automation engine which can help you to automate most of your repetitive tasks in your work life. Ansible can also improve the consistency, scalability, reliability and easiness of your IT environment. 

Ansible Cheat Sheet for Quick Reference and understanding

Visit [techbeatly.com](http://www.techbeatly.com) for more articles.

## Variables

| Item  | Description |
| ------------- | ------------- |
| host_vars  | directory for host variable files  |
| group_vars | directory for group variable files |
| facts | collecting the host specific data |
| `register` | registered variables |
| `vars` | in playbook |
| `vars_files` | in playbook |
| `include_vars` | module |
| `include_tasks: stuff.yml` | include a sub task file |

## Task Control & Loops

| Item  | Description |
| ------------- | ------------- |
| `with_items` | then “item” inside action |
| `with_nested` | for nested loops
| `with_file` | |
| `with_fileglob` | |  
| `with_sequence` | |
| `with_random_choice` | |
| `when` | meet a condition |


## Modules

| Item  | Description |
| ------------- | ------------- |
| `copy`  | copy file or content |
| `get_url`  | download file |
| `file`  | manage file/directories |
| `yum`  | manage package |
| `service`  | manage services |
| `firewalld`  | firewall service |
| `lineinfile`  | add a line to dest file |
| `template`  | to template file with variables |
| `debug` | to debug and display |
| `add_host` | add host to inventory while play |
| `wait_for` | use for flow control  |
| `apt`  | manage apt-packages |
| `shell` | execute shell commands on targets |

## Playbooks

| Item  | Description |
| ------------- | ------------- |
| `ansible-playbook <YAML>` | Run on all hosts defined |
| `ansible-playbook <YAML> -f 10` | Fork - Run 10 hosts parallel |
| `ansible-playbook <YAML> --verbose` | Verbose on successful tasks |
| `ansible-playbook <YAML> -C` | Test run |
| `ansible-playbook <YAML> -C -D` | Dry run |
| `ansible-playbook <YAML> -l <host>` | Limit to run on single host |


## Handlers

| Item  | Description |
| ------------- | ------------- |
| `notify` | to notify the handler |
| `handlers` | define handler |


## Tags

| Item  | Description |
| ------------- | ------------- |
| `tags` | add tags to the tasks |
| `--tags ‘<tag>’` | during playbook execution |
| `--skip-tags` | for skipping those tags |
| `tagged` | run any tagged tasks |
| `untagged` |any untagged items |
| `all` | all items |


## Handling Errors

| Item  | Description |
| ------------- | ------------- |
| `ignore_errors` | proceed or not if any error on current task |
| `force_handlers` | call handler even the play failed |
| `failed_when` | mark the task as failed if a condition met |
| `changed_when` | set  “ok” or “failed” for a task |
| `block` | logical grouping of tasks (can use with when) |
| `rescue` | to run if block clause fails |
| `always` | always run even block success or fails |

## Jinja2 Templates
*To be added later with examples*

## Roles
main file in sub-directories should be main.yml
Role variable can define under **roles** directive

**Role Directories**

| Item  | Description |
| ------------- | ------------- |
| defaults | default value of role variables |
| files | static files referenced by role tasks |
| handlers | role’s handlers |
| meta | role info like Author, Licence, Platform etc |
| tasks | role’s task defenition |
| templates | jinja2 templates |
| tests | test inventory and test.yml |
| vars | role’s variable values |
| pre_tasks | tasks before role |
| post_tasks | tasks after role |

## Ansible Galaxy
https://galaxy.ansible.com

| Item  | Description |
| ------------- | ------------- |
| `ansible-galaxy search ‘install git’ --platform el` | search for a role |
| `ansible-galaxy info <role-name>` | display role information |
| `ansible-galaxy install <role-name> -p <directory>` | install role from galaxy |
| `ansible-galaxy list` | to list local roles |
| `ansible-galaxy remove <role-name>` | remove role |
| `ansible-galaxy init --offline <role-name>` | initiate a role directory |

## Delegation

| Item  | Description |
| ------------- | ------------- |
| `delegate_to: localhost` | run the task on localhost instead of inventory item |
| `delegate_facts` | assign the gathered facts from the tasks to the delegated host instead of current host |

## Parallelism

| Item  | Description |
| ------------- | ------------- |
| `forks` | number of forks or parallel machines|
| `--forks` | when using ansible-playbook |
| `serial` | control number parallel machines |
| `async: 3600` | wait 3600 seconds to complete the task |
| `poll: 10` | check every 10 seconds if task completed |
| `wait_for` |module to wait and check  if specific condition met |
| `async_status` | module to check an async task status |


## Ansible Vault

| Item  | Description |
| ------------- | ------------- |
| `ansible-vault create newfile` | create a new vault file |
| `ansible-vault view newfile` | view file which is already ansible vaulted |
| `ansible-vault edit newfile` | Edit file |
| `ansible-vault view --vault-password-file .secret newfile` | Provide vault password as file |
| `ansible-vault decrypt newfile` | Remove encryption or vault |
| `ansible-vault rekey newfile` | change vault password |
| `--ask-vault-pass` or <br /> `--vault-password-file <secret-password-file>` | ask for vault password for ansible-playbook


## Troubleshooting

| Item  | Description |
| ------------- | ------------- |
| log_path | where logs are saved |
| `debug` | module for debugging |
| `--syntax-check` | syntax checking for playbooks before they run |
| `--step` | run playbook step by step |
| `--start-at-task`| run a playbook but start at specific task |
| `--check` | check mode |
| `--diff` | will show the expected changes if you run the playbook, but will not do any changes (kind of dry run) |
| `uri` | module for testing url |
| `script` | module for running script and return success code |
| `stat` | module to check the status of files/dir | 
| `assert` | check file exist |

*if you find any mistakes, please feel free to update or let me know*

Thanks

[LinkedIn](http://bit.ly/gineesh)  \|  [techbeatly.com](www.techbeatly.com)
