---
layout: post
title: Terraform
author: gini
categories: [ iac, cloud ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: [iac, terraform]
permalink: terraform
featured: false
hidden: false
titleshort: terraform
---

- [1. Introduction](#1-introduction)
  - [1.1. References to Start](#11-references-to-start)
- [2. Creating first Instance using Terraform](#2-creating-first-instance-using-terraform)
  - [2.1. Configure AWS Credential](#21-configure-aws-credential)
  - [2.2. Create your first terraform fie](#22-create-your-first-terraform-fie)
  - [2.3. Destroying Resource](#23-destroying-resource)
  - [2.4. Terraform DigitlOcean Droplet](#24-terraform-digitlocean-droplet)
  - [Terraform State File](#terraform-state-file)
  - [Desired State and Current State](#desired-state-and-current-state)
  - [Provider Architecture](#provider-architecture)
  - [Types of Terraform Provides](#types-of-terraform-provides)
    - [Configure 3rd Party provider](#configure-3rd-party-provider)
- [Managing Configurations](#managing-configurations)
  - [Understanding Attributes](#understanding-attributes)
- [Appendix A - Useful References](#appendix-a---useful-references)
- [Appendix B - Notes](#appendix-b---notes)

# 1. Introduction
## 1.1. References to Start

- [Get Started](https://learn.hashicorp.com/terraform)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-study)
- [Exam Review - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-review)

- [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- [Terraform Beginners Track](https://github.com/collabnix/terraform/blob/master/beginners/README.md) - (collabnix - GitHub)
- [Using Ansible to automate app deployment on Terraform-provided infrastructure](https://cloud.ibm.com/docs/terraform?topic=terraform-ansible)
- [Writing Ansible Playbooks for New Terraform Servers](https://victorops.com/blog/writing-ansible-playbooks-for-new-terraform-servers) - (victorops.com)
- [Terraform Course - Automate your AWS cloud infrastructure](https://www.youtube.com/watch?v=SLB_c_ayRMo) (freeCodeCamp.org)
- [HashiCorp Infrastructure Automation Certification](https://www.hashicorp.com/certification/terraform-associate/)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/terraform/certification/terraform-associate-study-guide)
- [HashiCorp Certified Terraform Associate - Overview](https://www.youtube.com/watch?v=vhZEdqlXlSs) (Video)
- [terraform-beginner-to-advanced-resource - GitHub](https://github.com/zealvora/terraform-beginner-to-advanced-resource)


```
provisioner "local-exec" {
  command = "sleep 120; ansible-playbook -i '${digitalocean_droplet.www-example.ipv4_address}' playbook.yml"
}
```

# 2. Creating first Instance using Terraform

-[Providers](https://www.terraform.io/docs/providers/index.html)

eg: AWS credential can give as,
- Static credentials
- Environment variables
- Shared credentials/configuration file
- CodeBuild, ECS, and EKS Roles
- EC2 Instance Metadata Service (IMDS and IMDSv2)

## 2.1. Configure AWS Credential
- IAM -> Users -> Create New user, Programatic Access
- Attach Exisiting Policies -> Add Administrator Access
- Take Access Key and Secret Key

## 2.2. Create your first terraform fie

```
provider "aws" {
  region     = "us-west-2"
  access_key = "my-access-key"
  secret_key = "my-secret-key"
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name = "HelloWorld"
  }
}
```

Then, 
- `terraform init` which will download and configure plugins which we have mentioned in the terraform file.
- `terraform plan` will show you the items going to create
- `terraform apply` will create the resources as per terraform template.

## 2.3. Destroying Resource

- `terraform destoy` will delete the resources
- `terraform destroy -target aws_instance.web` - destroy specific resource only.
- also you can comment out the resource, then terraform will detect it as not needed and will remove when you `plan` or `apply`

## 2.4. Terraform DigitlOcean Droplet
To generate API tokens from Digital Ocean

https://cloud.digitalocean.com/account/api/tokens

```
provider "digitalocean" {
  token = "PUT-YOUR-TOKEN-HERE"
}


resource "digitalocean_droplet" "kplabsdroplet" {
  image  = "ubuntu-18-04-x64"
  name   = "web-1"
  region = "nyc1"
  size   = "s-1vcpu-1gb"
}
```

## Terraform State File
- will keep every info about the resource

## Desired State and Current State

Terraform always make current state to desired state. 

- `terraform refresh` - will refresh the state by checking current state
- `terraform plan` will do `refresh` automatically in background.
- `terraform show` will give you the details of state (intstead of opening `.tfstate` file)
- Desired state always follow your `.tf` content.

## Provider Architecture

```
                                                       +-------------------+
+-------------+     +---------------+  +-----------+   |                   |
|             |     |               |  |           |   |   Resources       |
|  .tf file   +---->+   terraform   +->+ Provider  +-->+   in              |
|             |     |               |  |           |   |   Provider        |
+-------------+     +---------------+  +-----------+   |                   |
                                                       +-------------------+
```
- provider will have different versions, if nothing mentioned it will take the latest.
- for production, mention specific version of provider as needed. 
- 
```
>=1.0             Greater than equal to the version
<=1.0             Less than equal to the version
~>2.0             Any version in the 2.x range
>=2.10, <=2.30    Any version between 2.10 and 2.30
```

## Types of Terraform Provides
1. Hashicorp Distributed
   - Download automatically during `terraform init`
2. Third Party or Community providers
   - For cases where official providers not supporting some features.
   - for some proprierart platform to use with Terraform

### Configure 3rd Party provider
- Place the plugin in specific directory
  - Windows: `%APPDATA%\terraform.d\plugins`
  - All other systems: `~/.terraform.d/plugins`


# Managing Configurations

- keep configurations in directories

## Understanding Attributes

















# Appendix A - Useful References
- [Ansible, Terraform Excel Among Site Reliability Engineers, DevOps](https://thenewstack.io/ansible-terraform-excel-among-site-reliability-engineers-devops/)


# Appendix B - Notes

Declarative Infrastructure Management
- trackable via vcs
- automation CI/CD
- Reproducible env
- safe and predictable
- workflow
- opensource providers

