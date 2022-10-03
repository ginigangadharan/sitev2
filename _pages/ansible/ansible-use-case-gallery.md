---
layout: post
title: Ansible Automation - Use Case Gallery
author: gini
categories: [ ansible ]
image: "assets/images/2021/lenny-kuhne-jHZ70nRk7Ns-unsplash -automation.jpg"
tags: []
show-avatar: false
permalink: ansible-use-case-gallery
featured: false
hidden: false
titleshort: Ansible Use Cases
---

- [Immutable Infrastructure](#immutable-infrastructure)
- [Provisioning](#provisioning)
  - [AWS](#aws)
- [Application Deployment](#application-deployment)
- [Security Management](#security-management)
- [Manage network configurations](#manage-network-configurations)
- [Package Deployment](#package-deployment)
- [Operating System Patching](#operating-system-patching)
- [Weekly system reboot](#weekly-system-reboot)
- [Enforce security guidelines](#enforce-security-guidelines)
- [Configuration Management](#configuration-management)
- [Disaster recovery](#disaster-recovery)
- [Adhoc Commands](#adhoc-commands)
- [Database](#database)
  - [Ansible Oracles Modules](#ansible-oracles-modules)
  - [Database binary patching](#database-binary-patching)
- [Service license agreements](#service-license-agreements)
  
## Immutable Infrastructure

- Configure your re-buildable replicas of server set or application stack.
- Use Ansible for **Infrastructure as Code** (IaC)

## Provisioning

Provision your bare-metal, private cloud and public cloud infrastructure using modules and configure them to use for day 2 operations.

### AWS 

- Instance Provisioning
- LoadBalancer /  TargetGroup Creation
- VPC Creation

## Application Deployment

Manage your applications deployments, re-deployments, migration using simple ansible playbooks; and implement DevSecOps methods in your infrastructure operations.

## Security Management

- Incident Response

## Manage network configurations

- Configure new network devices with same configurations as we need
- Add new rules or changes in configurations (eg: new VLAN, new Virtual Interface)
- Add/Remove rule in firewall

## Package Deployment

- [Windows Package Management](https://www.ansible.com/blog/windows-package-management)

## Operating System Patching

- [Linux OS Patching Using Ansible](https://www.youtube.com/watch?v=sGXTfsz-3-4) (techbeatly)
- [Windows Updates with Ansible](http://www.vmlab.com.pl/windows-updates-and-ansible/)
- [Automate Windows updates with Ansible](https://4sysops.com/archives/automate-windows-updates-with-ansible/)
- [Windows Updates and Ansible](https://www.ansible.com/blog/windows-updates-and-ansible) (Ansible Blog)


## Weekly system reboot

Eliminate repetitive, manual processes with automation.


## Enforce security guidelines
Rules are rules. It’s best to automate in an effort to achieve strict security standards.

## Configuration Management

Keep your configurations as a code and also can check if someone made chages to the same.

## Disaster recovery
Disaster recovery can involve a wide range of components. Act across different variables of the technology stack to identify problems and eliminate cross team dependencies.

## Adhoc Commands
Remarkably easy to write, you can run commands across your environment for any number of servers.

## Database

### Ansible Oracles Modules

- [ansible-oracle-modules](https://github.com/oravirt/ansible-oracle-modules)

### Database binary patching
Several databases use outdated binary sets. Patch the binaries in accordance with the release of the latest patch.

## Service license agreements

- Gather subscription information and generate reports.
- Auto reminder emails on subscription and license renewal
-  


# Provisioning
Provisioning VMs on RHV, VMware and Hyper-V using templates
Provisioning on AWS
VM Snapshots on RHV and VMware

# Network 
Firewall rules orchestration with AlgoSec, Tuffin
Load Balancer orchestration with OS updates
SD-WAN
Integration with Infoblox (IPAM)
Asset management with Solarwinds

# Compliance and Security
CIS Scanning and Hardening for RHEL, SUSE and Windows
CIS benchmark for Kubernetes
Rapid 7 Nexpose / Tenable Nessus Integration
Anti-virus, e.g. Trend Micro, Symantec

# Application 
Integration with CI/CD tools such as Azure DevOps
Application Deployment
Installation and lifecycle management of middleware
Installation of IIS, MS-SQL
Installation of .NET app on Windows
Application health check
Dynatrace webhook
Integration with Jira ServiceDesk

# Config Management
RHEL and Windows patching with Red Hat Satellite and WSUS
Red Hat Clustering Setup
Health Check
Onboarding of Windows vm to Windows AD
Integration with CyberArk vault
DR failover for Ansible Tower
Golden Image management
Configuration drift management, with baseline configurations
