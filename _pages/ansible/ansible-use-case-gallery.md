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
- [OpenShift Container Platform - Automation](#openshift-container-platform---automation)
  - [OpenShift Day 2 Operations](#openshift-day-2-operations)
  - [Business Continuity](#business-continuity)
  - [Application Deployment/CD](#application-deploymentcd)
  - [Compliance](#compliance)
- [Operating System Patching](#operating-system-patching)
- [Weekly system reboot](#weekly-system-reboot)
- [Enforce security guidelines](#enforce-security-guidelines)
- [Disaster recovery](#disaster-recovery)
- [Adhoc Commands](#adhoc-commands)
- [Database and Middleware](#database-and-middleware)
  - [Ansible Oracles Modules](#ansible-oracles-modules)
  - [Database binary patching](#database-binary-patching)
- [Service license agreements](#service-license-agreements)
- [Infrastructure Provisioning](#infrastructure-provisioning)
- [Network](#network)
- [Application](#application)
- [Config Management](#config-management)
- [System and Server Automation](#system-and-server-automation)
- [Cloud Related Automation](#cloud-related-automation)
- [Operational Task](#operational-task)
  
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
- Backup of Fortigate Config into Tectia
- Failover and Failback of Fortigate
- Install Security Agents for Imperva, TrendMicro, McAfee, Splunk UF, Nessus, Carbon EDR
- CyberArk Credential retrievals
- Palo Alto Patching
- Palo Alto Provision/Deprovision and Firmware upgrades
- F5 Patching

## Manage network configurations

- Configure new network devices with same configurations as we need
- Add new rules or changes in configurations (eg: new VLAN, new Virtual Interface)
- Add/Remove rule in firewall

## Package Deployment

- [Windows Package Management](https://www.ansible.com/blog/windows-package-management)

## OpenShift Container Platform - Automation 

### OpenShift Day 2 Operations

- Cluster upgrade
- Auto-scaling of worker nodes
- AD group synchronization
- Time synchronization
- Housekeeping
- Pruning objects (namespaces, builds, etc.)
- Certificate update/rotation

### Business Continuity

- etcd backup
- Global Load Balancer (GTM) failover
- Cluster rebuild and recovery
- Master/Worker nodes VM snapshot

### Application Deployment/CD

- Namespace creation
- Network policy creation
- Application deployment, resource provisioning and testing
- Application upgrade

### Compliance 
- 
- CIS OpenShift scanning and reporting 
- Leveraging compliance operator and generating HTML reports


## Operating System Patching

- [Linux OS Patching Using Ansible](https://www.youtube.com/watch?v=sGXTfsz-3-4) (techbeatly)
- [Windows Updates with Ansible](http://www.vmlab.com.pl/windows-updates-and-ansible/)
- [Automate Windows updates with Ansible](https://4sysops.com/archives/automate-windows-updates-with-ansible/)
- [Windows Updates and Ansible](https://www.ansible.com/blog/windows-updates-and-ansible) (Ansible Blog)


## Weekly system reboot

Eliminate repetitive, manual processes with automated [weekly syste reboot](https://www.techbeatly.com/system-reboot-with-ansible-reboot-module/).

## Enforce security guidelines

Rules are rules. It’s best to automate in an effort to achieve strict security standards.

- CIS Scanning and Hardening for RHEL, SUSE and Windows
- CIS benchmark for Kubernetes
- Rapid 7 Nexpose / Tenable Nessus Integration
- Anti-virus, e.g. Trend Micro, Symantec

## Disaster recovery

Disaster recovery can involve a wide range of components. Act across different variables of the technology stack to identify problems and eliminate cross team dependencies.

## Adhoc Commands

Remarkably easy to write, you can run commands across your environment for any number of servers.

## Database and Middleware

- Installation and configuration of MySQL, MSSQL, Oracle DB, PostgreSQL, MariaDB
- Installation and configuration of WebLogic, Apache, NGINX, Tomcat, WebSphere, JBoss EAP
- DB health check for MySQL, MSSQL, Oracle DB
- DB housekeeping and archival for MySQL, Oracle DB
- DB ad-hoc backup/restore for MySQL, MSSQL, Oracle DB
- DB patching for MySQL, MSSQL, Oracle DB
- DB hardening and compliance for MySQL, MariaDB, MSSQL, PostgreSQL 
- Platform build/installation for Bamboo, Jira, Bitbucket, Confluence, minio, MQ etc
- Start/Stop/Restart DB and middleware services
- Patching for WebLogic, JBoss EAP, Tomcat

### Ansible Oracles Modules

- [ansible-oracle-modules](https://github.com/oravirt/ansible-oracle-modules)

### Database binary patching

Several databases use outdated binary sets. Patch the binaries in accordance with the release of the latest patch.

## Service license agreements

- Gather subscription information and generate reports.
- Auto reminder emails on subscription and license renewal

## Infrastructure Provisioning

- Provisioning VMs on RHV, VMware and Hyper-V using templates
- Provisioning on AWS
- VM Snapshots on RHV and VMware

## Network

- Firewall rules orchestration with AlgoSec, Tuffin
- Load Balancer orchestration with OS updates
- SD-WAN
- Integration with Infoblox (IPAM)
- Asset management with Solarwinds
- FW rules management (Add, delete and modify
- Cloud FW rules management (AWS and Azure)
- Compliance check for Cisco ASR, CSR, 9K, ACI, SD-WAN, Allot, F5, AlgoSec, Palo Alto, Panorama
- Hardening of Cisco ASR, CSR, 9K, ACI, SD-WAN, Allot, F5, AlgoSec, Palo Alto, Panorama
- Patch management for F5, InfoBlox, vEdge
- PKI Certificate management
- Configuration backup for Cisco IOS/XE, Palo Alto, F5, ACI
- Provisioning for vEdge, F5, ACI, Cisco IOS/XE, Palo Alto
- Health Check for Cisco IOS/XE, NX-OS, Palo Alto, F5
- Palo Alto user management, NSP, FW session, deprovisioning

## Application 

- Integration with CI/CD tools such as Azure DevOps
- Application Deployment
- Installation and lifecycle management of middleware
- Installation of IIS, MS-SQL
- Installation of .NET app on Windows
- Application health check
- Dynatrace webhook
- Integration with Jira ServiceDesk

## Config Management

Keep your configurations as a code and also can check if someone made changes to the same.

- RHEL and Windows patching with Red Hat Satellite and WSUS
- Red Hat Clustering Setup
- Health Check
- Onboarding of Windows vm to Windows AD
- Integration with CyberArk vault
- DR failover for Ansible Tower
- Golden Image management
- Configuration drift management, with baseline configurations


## System and Server Automation

- VM provision and deprovision for Windows 2016/2019, RHEL 8.x 
- Scale up/down CPU and Memory of VMs
- Add/Delete Virtual Disks for VMs
- Agent Installations for APM, McAfee, SCCM, Backup, Monitoring etc.
- Join to Domains
- Patch Management with WSUS/Satellite
- Compliance Management for RHEL, Windows
- Server Hardening
- Health Check for Windows and Linux Servers
- Server Stop/Start/Restart

 
## Cloud Related Automation

- VMs and Kubernetes provisioning on AWS/Azure
- Provisioning with Autoscaling Group on AWS
- Configure ALB
- Create Lambda Functions to update AMI
- VPC provision and deprovision with related network services
- Windows/Linux Patch Management
- Windows/Linux Hardening and Rollback Management
- Windows/Linux Compliance Management
- Golden Image Builds
- CloudWatch configurations
- Logs management/archival
- Lifecycle management of AWS accounts, organizations, roles and access
- Network security and WAF policy configurations
- AWS Resource backup
- Set budget limits and budget monitoring
- SFTP configurations with RBAC
- Cloud DB provisioning
- Azure SQL managed instance provisioning
- SNS configurations and alarm settings
- VAPT scanning


## Operational Task

- System Health Check for Windows, Linux, MySQL, Oracle DB
- Network Health Check for AlgoSec, Infoblox, ACI, Firepower, Router, F5, Palo Alto
- Server, DB and Application Services Stop/Start/Restart
- Common Incident Remediations
- DB Operational Tasks
