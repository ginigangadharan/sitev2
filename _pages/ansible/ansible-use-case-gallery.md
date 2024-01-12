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
- [Infrastructure Management](#infrastructure-management)
  - [VMWare](#vmware)
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

## Infrastructure Management

- Provisioning VMs on RHV, VMware and Hyper-V using templates.
- Provisioning on AWS.
- VM Snapshots on RHV and VMware.
- Maintain server consistency: Ensure all VMs are configured identically and adhere to company standards, promoting stability and security.

### VMWare

- **Datacenter and Cluster Management:**
   - Create and manage datacenters and clusters within vCenter.
   - Configure cluster settings for resource allocation, high availability, and security.
   - Optimize resource distribution across clusters for workload balancing.

- **Resource Pool Management:**
   - Create and manage resource pools to allocate CPU, memory, and storage resources effectively.
   - Set resource limits and shares for different workloads to ensure fair distribution.
   - Automatically adjust resource allocation based on dynamic needs.

- **Network Management:**
   - Configure virtual networks, switches, port groups, and VLANs within vCenter.
   - Manage network security policies and firewall rules to protect virtual environments.
   - Automate network provisioning and changes for new applications and environments.

- **Storage Management:**
   - Provision and manage datastores, including NFS, VMFS, and vSAN.
   - Create and manage storage policies for different VM requirements, such as performance and data protection.
   - Perform storage maintenance tasks like rescanning datastores and extending volumes.

- **Datastore Cluster Management:**
   - Create and manage datastore clusters for aggregated storage capacity and performance.
   - Enable Storage DRS for automatic load balancing of VMs across datastores within a cluster.
   - Configure Storage I/O Control (SIOC) to manage storage resources and prevent contention.

- **Content Library Management:**
   - Create and manage content libraries to store and distribute VM templates, ISO images, and other files.
   - Automate the deployment of VMs and applications from content libraries for consistency and efficiency.
   - Synchronize content libraries across multiple vCenter instances for multi-site deployments.

- **vSphere Update Manager (VUM) Integration:**
   - Automate patching and updating of ESXi hosts, virtual appliances, and VMware tools using Ansible and VUM.
   - Orchestrate compliance checks and remediation of vulnerabilities across the VMware environment.
   - Schedule and manage patching windows for minimal disruption to workloads.

- **vSphere Lifecycle Manager (vLCM) Integration (if available):**
   - Automate image-based management of ESXi hosts using Ansible and vLCM.
   - Enforce desired states for ESXi host configurations and firmware updates.
   - Simplify lifecycle operations and reduce manual intervention.

- **Role-Based Access Control (RBAC) Management:**
   - Automate the creation and assignment of roles and permissions within vCenter.
   - Enforce security policies and compliance requirements through role-based access.
   - Delegate administrative tasks to specific users or groups while maintaining control over privileges.

- **vSphere Alarms and Events Management:**
   - Configure vSphere alarms for proactive monitoring and alerting on critical events.
   - Integrate with external monitoring systems to trigger actions based on vSphere alarms.
   - Automate incident response workflows to address issues quickly and efficiently.

- Deploying VMWare Clusters.
- Building ESXi Servers and adding to vCenter.
- Cluster Configuration: Automate the configuration of VMware clusters in vCenter, ensuring consistent settings across your infrastructure.
- Resource Pool Management: Use Ansible to create, modify, or delete resource pools within vCenter to efficiently allocate and manage computing resources.
- Datastore Provisioning: Automate the provisioning and management of datastores on your ESXi hosts through vCenter, streamlining storage administration.
- Network Configuration: Manage network settings and configurations for ESXi hosts and vCenter using Ansible playbooks to ensure consistency and security.
- Template Deployment: Automate the deployment of virtual machine templates, making it easier to scale up or replace instances as needed.
- Distributed Switch Management: Use Ansible to configure and manage distributed switches in vCenter, simplifying network administration across multiple hosts.
- vSphere Update Manager: Automate the process of updating and patching ESXi hosts through vSphere Update Manager to ensure all hosts are running the latest software.
- Host Profiles and Configuration: Implement Ansible playbooks to manage host profiles, ensuring that ESXi hosts adhere to specific configuration standards.
- Alarm and Event Handling: Automate the setup and management of alarms and events within vCenter to receive notifications for critical events or issues.
- vCenter User Management: Use Ansible to manage user roles, permissions, and access controls within vCenter, maintaining a secure and organized virtual infrastructure.
- Managing vCenter resources such as nodes, storage, network.
- Patch management: Automate patching of operating systems and applications on VMs, increasing security and compliance.
- Patching and security compliance of vCenter and ESXi nodes.
- vCenter health check and reporting with dashboard and notification/alerts.
- 

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
