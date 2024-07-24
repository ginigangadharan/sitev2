---
layout: post
title: Ansible - Frequently Asked Questions
# 1. author: gini
categories: [ cloud ]
# 2. image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: [cloud, automation, containers, kubernetes]
show-avatar: false
permalink: ansible-faq
featured: false
hidden: false
titleshort: AWX
---

- [Ansible - Frequently Asked Questions](#ansible---frequently-asked-questions)
  - [What is Ansible?](#what-is-ansible)
  - [What is IaC (Infrastructure as Code)?](#what-is-iac-infrastructure-as-code)
- [AAP on OpenShift](#aap-on-openshift)
  - [What are user\_id will be created in each server/container . (Ex- In controller , hub , db and execution node )](#what-are-user_id-will-be-created-in-each-servercontainer--ex--in-controller--hub--db-and-execution-node-)
  - [What are file system and directory will be created/needed in each AAP component(server/container).](#what-are-file-system-and-directory-will-be-createdneeded-in-each-aap-componentservercontainer)
  - [Namespace requirement –Ie AAP need to be visible to all namespace or separate one only .](#namespace-requirement-ie-aap-need-to-be-visible-to-all-namespace-or-separate-one-only-)
  - [Number of persistent storage for each AAP component ( EX- controller , db ) with small summary of what information will be save on such storage .)](#number-of-persistent-storage-for-each-aap-component--ex--controller--db--with-small-summary-of-what-information-will-be-save-on-such-storage-)

## Ansible - Frequently Asked Questions

### What is Ansible?

Ansible is an open-source automation tool used for configuration management, application deployment, orchestration, and task automation. It uses human-readable YAML templates to describe automation jobs, making it easy for anyone to understand and write Ansible playbooks.

### What is IaC (Infrastructure as Code)?

Infrastructure as Code (IaC) is the process of managing and provisioning computing infrastructure through machine-readable definition files, rather than through physical hardware configuration or interactive configuration tools. IaC allows for automation and consistency in managing IT infrastructure, making it possible to deploy and manage infrastructure at scale.

**What can Ansible do?**

Ansible can:
- Automate repetitive IT tasks.
- Manage configurations on servers, network devices, and other infrastructure.
- Deploy applications across multiple environments.
- Orchestrate complex workflows involving multiple systems and tasks.
- Provision cloud resources on platforms like AWS, Azure, and Google Cloud.

**What are the advantages of Ansible?**

Simple and easy to learn: Uses YAML syntax, which is human-readable.
Agentless: No need to install any agents on the target machines.
Idempotent: Ensures that changes are made only when needed, preventing unintended consequences.
Scalable: Can manage thousands of nodes with minimal performance overhead.
Cross-platform: Works with various operating systems and devices.
How does Ansible work?
Ansible works by connecting to your nodes and pushing out small programs called "modules" to them. These modules are executed over SSH by default, and Ansible does not require any agent to be installed on the remote systems. After execution, the modules are removed. Ansible uses a "desired state" approach where the state of the system is described in playbooks, and Ansible ensures that the system reaches that state.

**What is a Playbook?**

A playbook is a YAML file that contains a series of tasks for Ansible to run. Playbooks are the core component of Ansible's configuration, deployment, and orchestration language. They describe the desired state of your systems in a simple and readable way, allowing you to define tasks, handlers, variables, and roles.

**Are there any requirements for using Ansible?**

Yes, the main requirements are:

- Python: Ansible requires Python to be installed on both the control machine and the managed nodes (usually Python 2.7 or Python 3.5+).
- SSH access: Ansible uses SSH to communicate with managed nodes, so SSH access must be configured and working.
- Privileges: Sufficient privileges to execute the tasks specified in the playbooks (sometimes root or sudo privileges).

**What is DevOps?**

DevOps is a set of practices that combines software development (Dev) and IT operations (Ops). The goal is to shorten the system development life cycle and provide continuous delivery with high software quality. DevOps emphasizes collaboration, automation, and integration between development and operations teams.

**How does Ansible fit into DevOps?**

Ansible fits into DevOps by automating the deployment and management of applications and infrastructure. It enables continuous integration and continuous delivery (CI/CD) pipelines, ensures consistency across environments, and reduces the time and effort required to manage infrastructure. Ansible's simplicity and flexibility make it a popular choice for DevOps teams.

**Who is Ansible for? Who should learn Ansible?**

Ansible is for:

- System administrators: To automate system configuration and management.
- DevOps engineers: To streamline CI/CD pipelines and manage infrastructure as code.
- Developers: To deploy applications and manage development environments.
- Network engineers: To automate network configuration and management.
- IT managers: To ensure consistency and efficiency in IT operations.

**What are prerequisites to learning Ansible?**

The prerequisites to learning Ansible are:

- Basic understanding of command-line operations.
- Familiarity with text editors (like Vim, Nano, or VS Code).
- Basic knowledge of system administration (especially Linux).
- Understanding of SSH and public/private key pairs.
- Familiarity with YAML syntax (helpful but not strictly necessary).

**What is Red Hat Ansible Inside**

[RHAI](https://access.redhat.com/products/red-hat-ansible-inside) is the Ansible components that allow application teams to embed Ansible playbook execution inside their application product.

## AAP on OpenShift

### What are user_id will be created in each server/container . (Ex- In controller , hub , db and execution node )

The AAP operator, controller, hub, execution and database pods are running using the official Red Hat container images. Most of them contains the `awx` user and other user account. There is no ready-to-share list of such information but you can find the following details under the container image page.

- Overview
- Security
- Technical Information
- Packages
- Dockerfile
- Get this image

Example URL for [Ansible Automation Platform controller(
ansible-automation-platform-23/controller-rhel8)](
https://catalog.redhat.com/software/containers/search?q=ansible-automation-platform-23&p=1) container image.

###  What are file system and directory will be created/needed in each AAP component(server/container).

These are container images and the files systems are pre-defined and no additional configurations is required. For the pods which required `persistentvolumes` (controller, hub, database etc), the required `persistentvolumeclaims` will be created by the AAP operator. This is customizable and users can define the size during the deployment. (Part of information gathering)

### Namespace requirement –Ie AAP need to be visible to all namespace or separate one only .

AAP Operator is can be installed in all namespaces but recommended to keep it in its own namespace.

### Number of persistent storage for each AAP component ( EX- controller , db ) with small summary of what information will be save on such storage .)

See the answer above.
