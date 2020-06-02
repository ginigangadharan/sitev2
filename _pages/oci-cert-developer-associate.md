---
layout: post
title: Oracle Cloud Infrastructure Developer 2020 Associate (1Z0-1084-20)
categories: [ Oracle ]
tags: [ OCI ]
show-avatar: false
permalink: oci-cert-developer-associate
featured: false
hidden: false
---
<!-- TOC orderedlist:false -->

- [Cloud Native Fundamentals](#cloud-native-fundamentals)
  - [CAP Theorem](#cap-theorem)
  - [Cloud Native Building Blocks](#cloud-native-building-blocks)
  - [Service Communication Patterns](#service-communication-patterns)
  - [Messaing Protcols](#messaing-protcols)
- [Oracle Functions Overview](#oracle-functions-overview)
- [Oracle Functions Core Concept](#oracle-functions-core-concept)
  - [Function Metrics](#function-metrics)
- [Oracle Functions - Use cases, References](#oracle-functions---use-cases-references)
- [API Gateway](#api-gateway)
  - [Use cases](#use-cases)
- [Resource Manager Overview](#resource-manager-overview)
- [Advanced Terraform](#advanced-terraform)
  - [Terraform Modules](#terraform-modules)
  - [Terraform provisioners](#terraform-provisioners)
- [OCI Streaming Services](#oci-streaming-services)
- [Oracle Kubernetes Engine (OKE)](#oracle-kubernetes-engine-oke)
- [Creating OKE Cluster](#creating-oke-cluster)
- [Accessing OKE Cluster using kubectl](#accessing-oke-cluster-using-kubectl)
- [OCI Service Broker](#oci-service-broker)
- [OCI API, SDK, CLI](#oci-api-sdk-cli)
- [Key Management Overview](#key-management-overview)
- [IAM Overview](#iam-overview)
  - [Authentication](#authentication)
  - [Autherization](#autherization)
- [IAM Mangement - Compartments](#iam-mangement---compartments)
- [IAM Mangement - Policies](#iam-mangement---policies)
- [IAM Mangement - Tags](#iam-mangement---tags)
- [Testing Cloud Native Applications](#testing-cloud-native-applications)
- [OCI Registry Service](#oci-registry-service)
- [Managing Repos in OCIR](#managing-repos-in-ocir)
- [Pulling an image from OCIR for Kubernetes](#pulling-an-image-from-ocir-for-kubernetes)
- [Image Retention Policies](#image-retention-policies)
- [Monitoring Service Overview](#monitoring-service-overview)
- [Events - Overview and Key Features](#events---overview-and-key-features)
- [Events - Core concepts](#events---core-concepts)
- [Events - Use cases, Reference Arch](#events---use-cases-reference-arch)

<!-- /TOC -->

## Cloud Native Fundamentals

Cloud Native vs Traditional Arch
- Stateful vs Statelsss
- Service Orchestration vs Service Chroreography
- Dealing with failures
  
### CAP Theorem
Distributed Database system
- reliable network 
- zero latency
- infinite bandwidth
- secure network
- no change in topology
- one admin
- zero transport cost
- homogenrous network

### Cloud Native Building Blocks

1. Microservices
   1. service oriented arch
   2. Loosly coupled services
   3. organized around business capabilties

**Defence in Depth Approach**
  - source code - track, audit, in repo
  - Container image - include only bare minimum needed
  - Container Registry - use private registry, vulnerability scanning (twistlock)
  - pods - image from approved registry, use pod security policies, host ports, networking 
  - Cluster/Orchestrator - secure access, Enable RBAC, Enable Audit logs


1. Containers
2. Functions

### Service Communication Patterns

- External Communication - to/from external services
- Internal Communication - service to service (within cluster)

- Sync vs Async Comm Protocols

- HTTP, HTTPv2, WebSocket, gRPC

### Messaing Protcols
- MQTT (Message Que Telemetry Transport)
- AMQP (Advance Message Queuing Protocol)

- Pub/Sub 
- Idempotency - multiple lines but single effect. message can process more than once


- Serialization
  - JSON - readable, large memory footprint
  - Protobuf - binary format, schema defined in .proto files

## Oracle Functions Overview

fnproject.io

- Function as a Service
- Oracle Cloud integrated
- Container Native
- Opensource Engine
- Multi tenant
- Secure

- run only when triggered
- pay for code execution only

- Function Development kit - FDK - Python, Java,Go, node.js, ruby

## Oracle Functions Core Concept
- grouped into applications
- built as a docker image and pushed to a specified docker registry

- invoke on cli, SDK, HTTP Req, other OCI services
- docker will be pulled and run; after some idle perioed container will be removed.
- Also time based functions
- define policy for permissions

### Function Metrics
- FunctionExecutionDuration
- FunctionInvocationCount
- FunctionResponseCount
  
## Oracle Functions - Use cases, References

- Glue Cloud Services, Event Driven
- Web, Mobile, IoT backends
- Realtime file, stream process
- DevOps, Batch process

## API Gateway

- single gateway to multiple API services
- Routing
- Rate limiting
- Cross region resource sharing (CORS)
- Metrics
- 

### Use cases
- RESTful API for functions
- Custom Development
- SaaS services

## Resource Manager Overview
- Terraform as a service
- Stack - a set of OCI resources you want to create in a compartment
- job - a request to take a terraform action on a stack (plan, apply, destroy)

## Advanced Terraform

- stores the state of managed ifnra from the last time terraform was run
- use this state to create plans and make changes to your infra
  terraform.tfstate

Terraform Local State File
- stored locally on local machine in JSON format
- small and individual team
- not to scale for large teams
- require a more mono-repo pattern

Terraform Remote State File
- write state to remote data store
- can manage by large team

- use target flag to create only that resource

### Terraform Modules

### Terraform provisioners
- ansible, che, puppet, shell

Instance Principal Config

## OCI Streaming Services

- logs, web/IoT/mobile data
- 99.95% SLA
- message - 64 bit encoded record or array of bytes
- key - and identifier
- stream - an append only log of messages
- topic - message category
- partitions - topic broken into partitions 
- producer - create message
- consumer - subscribe and read
- consumer group - a group 

Design Considerations
- retention max 7 days
- max message size 1 MB
- each partition 1000Emit API call per second, 5 Read API call per second
- each partition max total write rate 1 MB per second, read rate 2 MB per second
- each tenancy has a limit of 5 partitions 


## Oracle Kubernetes Engine (OKE)

- your own/DIY
- pre-built but managed by user
- managed service 

Terraform Kubernetes Installation for OCI

## Creating OKE Cluster
- monthly 3 cluster oer OCI regions, with 1000 nodes
- pay as you go model got only 1 cluster

## Accessing OKE Cluster using kubectl
- need oci cli
- then setup kubeconfig in oci cli
- 

- create serviceaccount with clusteradmin role to access dashboard 
- use token to login

## OCI Service Broker
- software to implement the Open service broker API
- enable cloud service lifecycle through devops tools (provision,bind,deprovision)
- specific to cloud vendor
- CI/CD -> OCI Registry -> OKE CLuster <- Service broker <- OCI Cloud services

## OCI API, SDK, CLI

Diff ways to access OCI
- GUI Console
- REST API - References and end points
- Terraform
- SDK- custom solutions based on java, python, ruby, go; need OCI account, user ID, key paid
- ANsible
- CLI - required keys and OCID. oci setup to configure, bash or pip install oci
- Resource Manager

## Key Management Overview
- managed service to encrypt data
- FIPS 140-2, Security level 3
- create keys, disable keys, re-enable keys with key vault
- rotate keys to meet your security governance and regulatory compliance needs; store in version
- with IAM users and groups

- vaults - logical entities to store keys
- seperate compartment for keys
- users/groups need access to keys

Design Considerations
- regional service, repicate keys across 3 AD
- block volumes and object storage are integrated with key managedment
- rotaing key will not re-encrypt data that was previously encrypted with old key version; only when customer modify data
- no import or export of keys
- cannot delete keys, bud disabe. delete key vault
- schedule vault deletion - 7-30 days
- cannot recover once deleted

pricing on vault - per hour

## IAM Overview
OCID - Oracle Cloud ID

Instance principal - for instance to make api calls


### Authentication
- authenticate principal by 
  - username /password
  - API Signining Key
  - Auth Tokens - no expire

### Autherization

- specified various actions an authenticated 

## IAM Mangement - Compartments
- sub compartments upto six level deep
- atleast one policy to access it
- subcompartments inherits access permisions from compartments higher up its hierarchy
  
Compartment Quotas
- similar to service limit but set by administrator using policies
- set, unset, zero

## IAM Mangement - Policies

Verbs
- inspect - ability to list
- read - inspect + get user specified metadata
- use - read + ability to work with existing resources
- manage - include all permission for resource

Aggregated - all-resource, database-family, instance-family etc
Individual - individual resource

Permissions
- VOLUME_INSPECT, READ, VOLUME_WRITE, 

API Operation
- ListVolume, GetVolume,CreateVolume
  
Common Policies

## IAM Mangement - Tags
Tag Namespace - a container for set of tag keys with tag key definitions

## Testing Cloud Native Applications

- mock - for testing
- fake - with working implementation
- stub - some data returns

Building block of testing
- unit tests
- service test/component level
- user interface tests

## OCI Registry Service

Oracle Cloud Infrastructure Registry - OCIR
- Fully managed
- HA Docker v2 registry
- private or public repo
- full integration with OKE

- OCI Registry is FREE
- Only charges for OCI resources

## Managing Repos in OCIR

- need proper permissions by policies
- users need username and auth token beore being able to push/pull image
- docker login <region-code>.ocir.io


## Pulling an image from OCIR for Kubernetes
- create a secret and use it as `imagePullSecret`

## Image Retention Policies
- not tagged for a certain period
- not pulled for a certain period
- not given particular tag

- Hourly process to reomve image
- policies to retain/remove images

## Monitoring Service Overview

- monitor cloud resources
- supports metrics and alarms
- compite, VCN, LB, block, object storage, notification, streaming
- health, capacity, performance

- email or pagerduty
- metric query language (MQL)

- metric is a measurement related to health, capacity or performance of a given resource

- metric stream - aggregated data of multiple metric data

- internval 01,5,60 min etc
- dimension - name-value pairs
- Grouping - groupBy() aggregates results by groups
- statistics - count, max, mean, rat, min, sum , percetile

Alarms & States
- Firing - alarm triggered
- Reset - alarm not detecting the metric firing, metric no longer being emitted
- Suppress - avoid publishing messages during the specified time range eg maintenance

metric + alarm + topic

## Events - Overview and Key Features

- fully managed event-routing platform
- using CNCF's [`cloudevent`](https://cloudevents.io/) (opensource) standard

- integration with oracle functons, streaming, notification


- Events - a structured and schematized message that denotes a change in resource
- Rule - the object where a user defines which event they care about and trgigger an actions an actions if it occurs
- Actions - the user-defined response to when an event occurs eg: triggering a functions or wrtingin to a stream, send notificaiton etc

## Events - Core concepts

name & compartment -> trigger condition -> Action

- max rules limit 50 / tenancy (can request more)

- user defined response to a rule being matched
- multiple actions can

## Events - Use cases, Reference Arch
