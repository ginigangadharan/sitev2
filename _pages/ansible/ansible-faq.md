---
layout: post
title: Ansible - Frequently Asked Questions
# author: gini
categories: [ Ansible, Automation ]
image: "assets/images/2020/ansible-automation.png"
tags: [ aap, cloud, automation, containers, kubernetes, ansible automation platform, red hat ansible]
# show-avatar: false
permalink: ansible-faq
featured: false
hidden: false
titleshort: Ansible FAQ
---

- [Ansible - Frequently Asked Questions](#ansible---frequently-asked-questions)
  - [What is Ansible?](#what-is-ansible)
  - [What is IaC (Infrastructure as Code)?](#what-is-iac-infrastructure-as-code)
  - [What can Ansible do?](#what-can-ansible-do)
  - [What are the advantages of Ansible?](#what-are-the-advantages-of-ansible)
  - [How does Ansible work?](#how-does-ansible-work)
  - [What is a Playbook?](#what-is-a-playbook)
  - [Are there any requirements for using Ansible?](#are-there-any-requirements-for-using-ansible)
  - [What is DevOps?](#what-is-devops)
  - [How does Ansible fit into DevOps?](#how-does-ansible-fit-into-devops)
  - [Who is Ansible for? Who should learn Ansible?](#who-is-ansible-for-who-should-learn-ansible)
  - [What are prerequisites to learning Ansible?](#what-are-prerequisites-to-learning-ansible)
  - [What is the Ansible AWX Project?](#what-is-the-ansible-awx-project)
  - [What is the difference between Ansible Core and AWX?](#what-is-the-difference-between-ansible-core-and-awx)
  - [Where can I find support for AWX?](#where-can-i-find-support-for-awx)
  - [What’s the difference between AWX and automation controller?](#whats-the-difference-between-awx-and-automation-controller)
  - [Can I upgrade from one version of AWX to another?](#can-i-upgrade-from-one-version-of-awx-to-another)
  - [Under which open source license is AWX available?](#under-which-open-source-license-is-awx-available)
  - [How can I get involved with the AWX Project?](#how-can-i-get-involved-with-the-awx-project)
  - [Where do I report an AWX bug?](#where-do-i-report-an-awx-bug)
  - [How often is AWX released?](#how-often-is-awx-released)
  - [What is the governance structure of the AWX project?](#what-is-the-governance-structure-of-the-awx-project)
  - [If my software runs with AWX, can I say that it is certified to run on AWX or on Red Hat Ansible Automation Platform?](#if-my-software-runs-with-awx-can-i-say-that-it-is-certified-to-run-on-awx-or-on-red-hat-ansible-automation-platform)
  - [I want to build my own forked version of AWX. Can I call it AWX? Can I call it Red Hat Ansible Automation Platform?](#i-want-to-build-my-own-forked-version-of-awx-can-i-call-it-awx-can-i-call-it-red-hat-ansible-automation-platform)
  - [Will contributions to AWX require a “Contributor License Agreement”?](#will-contributions-to-awx-require-a-contributor-license-agreement)
  - [Does Red Hat recommend AWX for production environments?](#does-red-hat-recommend-awx-for-production-environments)
  - [Does Red Hat’s Open Source Assurance Program apply to AWX?](#does-red-hats-open-source-assurance-program-apply-to-awx)
  - [What is Red Hat Ansible Inside (RHAI)](#what-is-red-hat-ansible-inside-rhai)
  - [What is Private Partner Automation Hub (PPAH)](#what-is-private-partner-automation-hub-ppah)
- [AAP on OpenShift](#aap-on-openshift)
  - [What are user\_id will be created in each server/container . (Ex- In controller , hub , db and execution node )](#what-are-user_id-will-be-created-in-each-servercontainer--ex--in-controller--hub--db-and-execution-node-)
  - [What are file system and directory will be created/needed in each AAP component(server/container).](#what-are-file-system-and-directory-will-be-createdneeded-in-each-aap-componentservercontainer)
  - [Namespace requirement –Ie AAP need to be visible to all namespace or separate one only .](#namespace-requirement-ie-aap-need-to-be-visible-to-all-namespace-or-separate-one-only-)
  - [Number of persistent storage for each AAP component ( EX- controller , db ) with small summary of what information will be save on such storage .)](#number-of-persistent-storage-for-each-aap-component--ex--controller--db--with-small-summary-of-what-information-will-be-save-on-such-storage-)

## Ansible - Frequently Asked Questions

**Disclaimer:**

This FAQ is a compilation of questions and answers gathered from various discussions. It also incorporates information from the [official Ansible FAQ](https://www.ansible.com/faq/). The answers are provided to the best of our ability based on currently available information. While we strive for accuracy, there's always a chance for improvement. If you have any further questions, discover an inaccuracy, or have additional topics you'd like covered, please don't hesitate to reach out to [gini@iamgini.com](mailto:gini@iamgini.com).

### What is Ansible?

Ansible is an open-source automation tool used for configuration management, application deployment, orchestration, and task automation. It uses human-readable YAML templates to describe automation jobs, making it easy for anyone to understand and write Ansible playbooks.

### What is IaC (Infrastructure as Code)?

Infrastructure as Code (IaC) is the process of managing and provisioning computing infrastructure through machine-readable definition files, rather than through physical hardware configuration or interactive configuration tools. IaC allows for automation and consistency in managing IT infrastructure, making it possible to deploy and manage infrastructure at scale.

### What can Ansible do?

Ansible can:
- Automate repetitive IT tasks.
- Manage configurations on servers, network devices, and other infrastructure.
- Deploy applications across multiple environments.
- Orchestrate complex workflows involving multiple systems and tasks.
- Provision cloud resources on platforms like AWS, Azure, and Google Cloud.

### What are the advantages of Ansible?

- Simple and easy to learn: Uses YAML syntax, which is human-readable.
- Agentless: No need to install any agents on the target machines.
- Idempotent: Ensures that changes are made only when needed, preventing unintended consequences.
- Scalable: Can manage thousands of nodes with minimal performance overhead.
- Cross-platform: Works with various operating systems and devices.

### How does Ansible work?

Ansible works by connecting to your nodes and pushing out small programs called "modules" to them. These modules are executed over SSH by default, and Ansible does not require any agent to be installed on the remote systems. After execution, the modules are removed. Ansible uses a "desired state" approach where the state of the system is described in playbooks, and Ansible ensures that the system reaches that state.

### What is a Playbook?

A playbook is a YAML file that contains a series of tasks for Ansible to run. Playbooks are the core component of Ansible's configuration, deployment, and orchestration language. They describe the desired state of your systems in a simple and readable way, allowing you to define tasks, handlers, variables, and roles.

### Are there any requirements for using Ansible?

Yes, the main requirements are:

- Python: Ansible requires Python to be installed on both the control machine and the managed nodes (usually Python 2.7 or Python 3.5+).
- SSH access: Ansible uses SSH to communicate with managed nodes, so SSH access must be configured and working.
- Privileges: Sufficient privileges to execute the tasks specified in the playbooks (sometimes root or sudo privileges).

### What is DevOps?

DevOps is a set of practices that combines software development (Dev) and IT operations (Ops). The goal is to shorten the system development life cycle and provide continuous delivery with high software quality. DevOps emphasizes collaboration, automation, and integration between development and operations teams.

### How does Ansible fit into DevOps?

Ansible fits into DevOps by automating the deployment and management of applications and infrastructure. It enables continuous integration and continuous delivery (CI/CD) pipelines, ensures consistency across environments, and reduces the time and effort required to manage infrastructure. Ansible's simplicity and flexibility make it a popular choice for DevOps teams.

### Who is Ansible for? Who should learn Ansible?

Ansible is for:

- System administrators: To automate system configuration and management.
- DevOps engineers: To streamline CI/CD pipelines and manage infrastructure as code.
- Developers: To deploy applications and manage development environments.
- Network engineers: To automate network configuration and management.
- IT managers: To ensure consistency and efficiency in IT operations.

### What are prerequisites to learning Ansible?

The prerequisites to learning Ansible are:

- Basic understanding of command-line operations.
- Familiarity with text editors (like Vim, Nano, or VS Code).
- Basic knowledge of system administration (especially Linux).
- Understanding of SSH and public/private key pairs.
- Familiarity with YAML syntax (helpful but not strictly necessary).

### What is the Ansible AWX Project?

The [Ansible AWX](https://github.com/ansible/awx) project is an open-source community initiative sponsored by Red Hat® that allows users to manage their Ansible projects in IT environments. It serves as the upstream project for the automation controller in Red Hat Ansible Automation Platform.

### What is the difference between Ansible Core and AWX?

**Ansible Core** is the command-line tool installed from community or official Red Hat repositories, focusing on core automation functionalities. It succeeded the "batteries included" ansible CLI package before Ansible 2.10, excluding many modules now found in collections.

**AWX** provides a web-based interface, REST API, and task engine built on top of Ansible. It's the upstream project for Red Hat's automation controller (formerly Ansible Tower), part of the Red Hat Ansible Automation Platform.

### Where can I find support for AWX?

Support for AWX is available through the Ansible Community Forum and Matrix. Users can also report bugs or contribute fixes on the [AWX GitHub repository](https://github.com/ansible/awx). Red Hat does not offer paid support for AWX; for a supported platform, consider [Red Hat Ansible Automation Platform](https://www.redhat.com/en/technologies/management/ansible).

### What’s the difference between AWX and automation controller?

AWX is a rapidly evolving project where new developments occur frequently. The [automation controller](https://www.redhat.com/en/technologies/management/ansible/automation-controller) is a hardened, supported version of AWX, offered as part of the Red Hat Ansible Automation Platform. This model is similar to Fedora's relationship with Red Hat Enterprise Linux®.

### Can I upgrade from one version of AWX to another?

Upgrades for AWX are supported only for installations using the awx-operator on Kubernetes. Follow the official upgrade instructions for awx-operator versions 0.18 or above. Direct, in-place upgrades between prior AWX versions are not supported.

### Under which open source license is AWX available?

AWX is licensed under the Apache License 2.0.

### How can I get involved with the AWX Project?

The AWX Project is hosted on GitHub, welcoming community contributions. Interested individuals can read the contributor guide and join the Ansible Community Forum.

### Where do I report an AWX bug?

Issues with AWX can be reported on its GitHub issue tracking page.

### How often is AWX released?

AWX aims to release new builds approximately every two weeks, with certain builds flagged as "stable" at the team's discretion. However, "stable" does not imply production readiness or any warranty.

### What is the governance structure of the AWX project?

Currently, the Red Hat sponsored AWX team makes most decisions, with community input. Over time, the governance structure may evolve to balance community and product needs.

### If my software runs with AWX, can I say that it is certified to run on AWX or on Red Hat Ansible Automation Platform?

Only Red Hat can certify software. While you can state that your software works with AWX, you cannot claim it is certified. Refer to the AWX trademark guidelines for details.

### I want to build my own forked version of AWX. Can I call it AWX? Can I call it Red Hat Ansible Automation Platform?

You may fork AWX, but you cannot use Red Hat trademarks for your fork. Red Hat has exclusive rights to these marks. Refer to the AWX trademark guidelines for more information.

### Will contributions to AWX require a “Contributor License Agreement”?

No, but contributors must agree to the Developer Certificate of Origin (DCO) upon submission. The DCO can be read at developercertificate.org.

### Does Red Hat recommend AWX for production environments?

No, Red Hat does not recommend AWX for production use.

### Does Red Hat’s Open Source Assurance Program apply to AWX?

No, it does not.

### What is Red Hat Ansible Inside (RHAI)

[RHAI](https://access.redhat.com/products/red-hat-ansible-inside) is the Ansible components that allow application teams to embed Ansible playbook execution inside their application product.

### What is Private Partner Automation Hub (PPAH)

The [**Private Partner Automation Hub**](https://connect.redhat.com/en/blog/private-partner-automation-hub) offering enables partners to protect their investment in Ansible automation, giving them a platform to be able to serve up their property with greater security and scalability capabilities. Using the Private Partner Automation Hub offering, the partner has the tools to curate their content and serve it to their target audience  of Red Hat Ansible customers.

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
