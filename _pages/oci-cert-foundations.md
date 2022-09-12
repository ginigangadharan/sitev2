---
layout: post
title: Oracle Cloud Infrastructure Foundations
categories: [ Oracle ]
tags: [ OCI ]
show-avatar: false
permalink: oci-cert-foundations
featured: false
hidden: false
---
<!-- TOC -->

- [Start OCI](#start-oci)
  - [Cloud Concepts](#cloud-concepts)
  - [OCI Architecture](#oci-architecture)
- [Core OCI Services](#core-oci-services)
  - [OCI Computer Services](#oci-computer-services)
  - [OCI Storage Services](#oci-storage-services)
  - [OCI Network Services](#oci-network-services)
    - [VCN to VCN Comms](#vcn-to-vcn-comms)
    - [Load Balancer](#load-balancer)
  - [OCI IAM](#oci-iam)
    - [Principals](#principals)
    - [Compartment](#compartment)
    - [Authentication](#authentication)
    - [Authorization](#authorization)
  - [OCI Database Services](#oci-database-services)
    - [OCI DB Options](#oci-db-options)
  - [OCI Security](#oci-security)
    - [Shared Security Model](#shared-security-model)
    - [Federation](#federation)
    - [Data Protection](#data-protection)
    - [Key Management](#key-management)
    - [OS Management Service](#os-management-service)
    - [Network Protection](#network-protection)
    - [OCI Web Application Firewall](#oci-web-application-firewall)
    - [Compliance Certifications](#compliance-certifications)
  - [OCI Pricing and Billing](#oci-pricing-and-billing)
    - [Pricing Models](#pricing-models)

<!-- /TOC -->
Exam : Oracle Cloud Infrastructure Foundations 2020 Associate (1Z0-1085-20)
105 Minuts/60 Questions/ 68% to pass

# Start OCI

## Cloud Concepts

- 21 Regions
- AD - Availability Domains, isolated each other
- within region 3 AD (some 2 or 1)
- FD - Fault Domain (Logical Domain)

## OCI Architecture
- Compartment - logical collection of resources

# Core OCI Services 

## OCI Computer Services

- baremetal
- Dedicated VM
- VM
- COntainer Enginer
- Functions

## OCI Storage Services

- block volume
  - Highly Durable - 3 replicas
  - Tiers - can be upto 32 GB (and maximum 32 volumes/disks)
    - Basic
    - Balanced
    - High Performance
    - 
- local NVMe - temp storage, non-persistent
  - high performance
  - 
- File Storage - files
  - NFS and SMB
  - Highly durable
  - can take snapshot 
- Object Storage
  - all in root folder, no folder
  - unstrctured 
  - hot
    - standard storage tier
  - cold
    - archive, backup
    - 10x cheaper than standard
    - 90 days retention minimum
    - 4 hr recovery/retrieve

## OCI Network Services

- VCN - Virtual Cloud Network
- Internet Gateway - 2 way
- NAT Gateway - only inwards
- Public Subnet /DMZ
- DRG - Virtual Router to connect On-Prem or Other destination
  - IPSec
  - Fast Connect
- Service Gateway - to connect internal objects like Storage; connection via VCN
- Firewall rules to restrict traffic (port/subnet)
  - Ingress
  - Egress
- Network Security Group - NSG apply to VCN

### VCN to VCN Comms
- VCN Peering
  - Local VCN Peering - same regions
  - Remoet VCN Peering - different regions
  - Peering is not transit; need dedicated peering

### Load Balancer
- Service Discovery
- Health Check
- Algorithm

## OCI IAM

### Principals

IAM entity that allowed to interact with OCI resources. 
- IAM Users and Groups
- Instance Principal - for instances, API, etc

### Compartment
- Collection of resources

### Authentication

- Who 
- OCI IAM service authenticate a Principal by
  - username, password
  - API Signing Key
  - Auth Tokens

### Authorization
- Specifies various actions an authenticated principal can do
- OCI Authorization = policies
- Written in Human-readable format
  - Allow group GROUP_NAME to VERB RESOURCE_TYPE in tenancy
    - verb - read, inspect, use, manage
    - resource type -  all,database, instance,virtual network etc
    - 
- Policy attachment - attach to compartment or tenancy
  
## OCI Database Services

### OCI DB Options
- VM DB systems - DB running on VM
- Bare Metal - on physical server
- Oracle RAC - Cluster, HA
- Exadata DB
- Autonomous - share
- Autonomous - dedicated
  - Self driving, Self securing, Self repairing

Operations
  - Start, stop, reboot
  - Scale - CPU, Storage
  - Patch - 2 steps process, DB first, then system.
    - Exadata and RAC - rolling patching

Backup/Restore
  - Manual or Auto
  - Retention period

DR Systems
  - Oracle Data Guard
  - Active Data Guard - extends data guard by providing advanced features for data protection and availability
  - Switchover - Planned migratons, no data losss
  - Failover - unplanned, minimal data loss

DB Systems HA and DR
  - primary and standby can be single instance or RAC

Autonomous DB
  - Autonomous Transation Process (ATP)
  - Autonomous Data Warehouse (ADW)


## OCI Security

### Shared Security Model
- OCI manage infra
- Customer manage VMs and Apps

### Federation

### Data Protection

- Data encrypted at-rest
- Data encrypted in-transit
- Bring your own keys

on
- block volume
- file storage
- object storage - pre-auth requests
- database - transparent data encryption, data safe, data vault

### Key Management

- BYOK - Bring your own keys
- HSM (hardware security modules)

- data safe

### OS Management Service
  - execute and automate common and complex management tasks
  - package management, config management
  - security and compliance


### Network Protection

- Tiered subnet strategy
  - DMZ
  - Public
  - Private

- Gateways
  - NAT
  - Service
  - Dynamic Routing

- Security List, NSG

### OCI Web Application Firewall

### Compliance Certifications

## OCI Pricing and Billing

### Pricing Models
- Pay as you go (PAYG)
- Monthly Flex
- Bring your own lincense

