---
layout: post
title: Terraform
author: gini
categories: [ iac, cloud ]
image: "assets/images/2020/infrastructure.jpg"
tags: [iac, terraform]
permalink: terraform
featured: false
hidden: false
titleshort: terraform
---

- [Introduction](#introduction)
  - [References to Start](#references-to-start)
- [Creating first Instance using Terraform](#creating-first-instance-using-terraform)
  - [Configure AWS Credential](#configure-aws-credential)
  - [Create your first terraform fie](#create-your-first-terraform-fie)
  - [Destroying Resource](#destroying-resource)
  - [Terraform DigitlOcean Droplet](#terraform-digitlocean-droplet)
  - [Terraform State File](#terraform-state-file)
  - [Desired State and Current State](#desired-state-and-current-state)
  - [Provider Architecture](#provider-architecture)
  - [Types of Terraform Provides](#types-of-terraform-provides)
    - [Configure 3rd Party provider](#configure-3rd-party-provider)
- [Managing Configurations](#managing-configurations)
  - [Understanding Attributes & Outputs](#understanding-attributes--outputs)
  - [Referencing Cross-Account Resource attributes](#referencing-cross-account-resource-attributes)
  - [Terraform Variables](#terraform-variables)
  - [Variable Assignment](#variable-assignment)
  - [Datatypes for Variables](#datatypes-for-variables)
  - [Count and Count Index](#count-and-count-index)
  - [Conditional Expression](#conditional-expression)
  - [Local Values](#local-values)
  - [Terraform Functions](#terraform-functions)
  - [Data Sources](#data-sources)
  - [Debugging in Terraform](#debugging-in-terraform)
  - [Terraform Format](#terraform-format)
  - [Validate Terraform File](#validate-terraform-file)
  - [Load Order & Semantics](#load-order--semantics)
  - [Dynamic Blocks](#dynamic-blocks)
  - [Terraform Taint](#terraform-taint)
- [Appendix A - Useful References](#appendix-a---useful-references)
- [Appendix B - Notes](#appendix-b---notes)
- [Appendix C - Frequently Asked Questions](#appendix-c---frequently-asked-questions)

# Introduction
## References to Start

- [Get Started](https://learn.hashicorp.com/terraform)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-study)
- [Exam Review - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-review)

- [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- [Terraform Beginners Track](https://github.com/collabnix/terraform/blob/master/beginners/README.md) - (collabnix - GitHub)
- [Using Ansible to automate app deployment on Terraform-provided infrastructure](https://cloud.ibm.com/docs/terraform?topic=terraform-ansible)
- [Writing Ansible Playbooks for New Terraform Servers](https://victorops.com/blog/writing-ansible-playbooks-for-new-terraform-servers) - (victorops.com)
- [What Is Immutable Infrastructure?](https://www.digitalocean.com/community/tutorials/what-is-immutable-infrastructure)
- [What is Mutable vs. Immutable Infrastructure?](https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure/)
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

# Creating first Instance using Terraform

-[Providers](https://www.terraform.io/docs/providers/index.html)

eg: AWS credential can give as,
- Static credentials
- Environment variables
- Shared credentials/configuration file
- CodeBuild, ECS, and EKS Roles
- EC2 Instance Metadata Service (IMDS and IMDSv2)

## Configure AWS Credential
- IAM -> Users -> Create New user, Programatic Access
- Attach Exisiting Policies -> Add Administrator Access
- Take Access Key and Secret Key

## Create your first terraform fie

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

## Destroying Resource

- `terraform destoy` will delete the resources
- `terraform destroy -target aws_instance.web` - destroy specific resource only.
- also you can comment out the resource, then terraform will detect it as not needed and will remove when you `plan` or `apply`

## Terraform DigitlOcean Droplet
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

```
>=1.0             Greater than equal to the version
<=1.0             Less than equal to the version
~>2.0             Any version in the 2.x range
>=2.10, <=2.30    Any version between 2.10 and 2.30
```

**Example**

```
terraform{
required_version = "~>0.12.0"

required_providers {
  aws = "~>2.6"  # is equivalent to >= 2.6, < 2.7
}
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

## Understanding Attributes & Outputs

Get details of created resources and use it for further steps.

```
resource "aws_s3_bucket" "mys3" {
  bucket = "demo-onboarding-20200903"
}

output "mys3bucket" {
  value = aws_s3_bucket.mys3.bucket_domain_name
}
```

then output will be,
```
.
.
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

eip = 13.251.177.36
mys3bucket = demo-onboarding-20200903.s3.amazonaws.com
```

- There are more attributes we can use; check documentation and under **Attribute Refernce** in each resource type.
- If we dont mention attribute, then terraform will display all attribues associated with the resource.

## Referencing Cross-Account Resource attributes

You can associate resources by referring attributes of resources.

Eg: Assigning EIP to an instance

```
resource "aws_eip_association" "eip_assoc" {
  instance_id =  aws_instance.web.id
  allocation_id = aws_eip.mylb.id
}
```

## Terraform Variables

- Create variables and store values for repeated usage

```
variable "my_ip" {
  default = "10.1.10.10/32"
}
```

and use the variable,
```
resource "aws_security_group" "allow_tls" {
  name        = "test-allow_tls"
  ingress {
    description = "TLS from VPC"
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = [var.my_ip]
  }

}
```

## Variable Assignment

1. Environment variables - can use environment variable with a prefix `TF_VAR_`. 
   eg: `export TF_VAR_instance_type=t2.micro`

2. Command Line Flags
   
   `terraform plan -var="instancetype=t2.small"`

3. From a File - use `terraform.tfvars` - terraform will load all variables from this file. 
   - if different var files to be used, then `terraform plan -var-file="custom.tfvars`
   
4. Variable Defaults - can keep variable default in another `.tf` file.
   ```
   $ cat variables.tf 
  variable "my_ip" {
    default = "10.1.10.10/32"
  }
  ```

- if no value mentioned, then `default` value will be used.
- if `default` value not defined, then terraform will ask for variable when do `apply` or `plan`

## Datatypes for Variables

Restict to use specific variable type

- number
- string
- list
- map 

variables.tf: 
```
variable "image_id" {
  type = string   
}

variable "az" {
  type = list
}

variable "list"  {
  type = list
  defaults = ["m5.large","m5.xlarge","t2.micro"]
}

variable "types"  {
  type = map
  defaults = {
    us-east-1 = "t2.micro"
    us-west-2 = "t2.nano"
    ap-south-1 = "t2.small"
  }
}

```

and `terraform.tfvars`
```
elb_name="myelb"
timeout="400"
az=["us-west-1a","us-west-1b"]
```

**Fetching Data from maps and list**

You can call variable as 
 - `instance_type = var.types["us-west-1a"]` - for a map
 - `instance_type = var.list[0]` - for list

## Count and Count Index

- Create multipe resources of same type
- use `count` parameter
- use `count.index` for counts and use it for indentifying resource. 

```
resource "aws_instance" "multi-instance" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = "t2.micro"
  count = 3

  tags = {
    Name = "hello-${count.index}"
  }
}
```

Also use the `count.index` to fetch details from a `list` and use it from names.
```
variable "instance_names"  {
  type = list
  default = ["web-front","web-back","db"]
}
```

and, 
```
  Name = var.instance_names[count.index]
```

## Conditional Expression

- Terraform create and act based on conditional expressions

```
variable "istest" {}

resource "aws_instance" "prod" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = "t2.micro"
  # if var.istest is false, then create 1 instance, else 0 instance
  count = var.istest == false ? 1 : 0
}

resource "aws_instance" "dev" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = "t2.large"
  # if var.istest is true, then create 1 instance, else 0 instance
  count = var.istest == true ? 1 : 0
}
```
then, mention your default value in `terraform.tfvars`

```
istest = true
```

## Local Values

[Doc - Local Values](https://www.terraform.io/docs/configuration/locals.html)

- define and use inside resources

```
locals {
  common_tags = {
    Owner = "Dev Team"
    Service = "Backend"
  }
}

# in resource
resource "aws_instance" "dev" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = "t2.large"
  tags = local.common_tags
}

resource "aws_ebs_volume" "db_ebs" {
  availability_zone = "us-west-2a"
  size              = 8
  tags = local.common_tags
}

``` 

## Terraform Functions

[Doc - Built-in Functions](https://www.terraform.io/docs/configuration/functions.html)

`function (argument1, argument2)`

- Test functins by `terraform console`
  

```
locals {
  time = formatdate("DD MMM YYYY hh:mm ZZZ", timestamp())
}

variable "region" {
  default = "ap-south-1"
}

variable "tags" {
  type = list
  default = ["firstec2","secondec2"]
}

variable "ami" {
  type = map
  default = {
    "us-east-1" = "ami-0323c3dd2da7fb37d"
    "us-west-2" = "ami-0d6621c01e8c2de2c"
    "ap-south-1" = "ami-0470e33cd681b2476"
  }
}

resource "aws_key_pair" "loginkey" {
  key_name   = "login-key"
  public_key = file("${path.module}/id_rsa.pub")
}

resource "aws_instance" "app-dev" {
   ami = lookup(var.ami,var.region)
   instance_type = "t2.micro"
   key_name = aws_key_pair.loginkey.key_name
   count = 2

   tags = {
     Name = element(var.tags,count.index)
   }
}
```

## Data Sources

[Doc - Data Source](https://www.terraform.io/docs/configuration/data-sources.html)

- Allow data to be fetched from external sources

```
data "aws_ami" "app_ami" {
  most_recent = true
  owners = ["amazon"]

  filter {
    name = "name"
    values = ["amzn2-ami-hvm*"]
  }
}

# then use the data variable in resource
resource "aws_instance" "app-dev" {
   ami = data.aws_ami.app_ami.id 
   instance_type = "t2.micro"
}
```

## Debugging in Terraform

[Doc](https://www.terraform.io/docs/internals/debugging.html)

- enable `TF_LOG` variable with appropriate values - `TRACE`, `DEBUG`, `INFO`, `WARN` or `ERROR`
- `export TF_LOG=TRACE` to see details logs
- `export TF_LOG_PATH=YOUR_PATH_FOR_LOG` will save logs in file

## Terraform Format

- Use `terraform fmt` to cleanup the code

## Validate Terraform File

- `terraform validate`

## Load Order & Semantics

- Terraform generally loads all the config files - `.tf` & `.tf.json` - within the directory, specified in alphabetical order.
- Split the code into multiple files, eg:
  - provider.tf
  - variables.tf
  - ec2.tf
  - iam.tf etc.

## Dynamic Blocks

[Doc](https://www.terraform.io/docs/configuration/expressions.html)

- for repeated steps or like loops

Normal blocks:

```
resource "aws_security_group" "demo_sg" {
  name        = "sample-sg"

 ingress {
    from_port   = 8200
    to_port     = 8200
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 8201
    to_port     = 8201
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

With dynamic blocks:

```
variable "sg_ports" {
  type        = list(number)
  description = "list of ingress ports"
  default     = [8200, 8201,8300, 9200, 9500]
}

resource "aws_security_group" "dynamicsg" {
  name        = "dynamic-sg"
  description = "Ingress for Vault"

  dynamic "ingress" {
    for_each = var.sg_ports
    iterator = port
    content {
      from_port   = port.value
      to_port     = port.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}
```

## Terraform Taint


















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

# Appendix C - Frequently Asked Questions

**Q. Is it mandatory to keep the variables in `variables.tf` ?**

