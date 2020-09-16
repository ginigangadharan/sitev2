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
  - [Terraform DigitalOcean Droplet](#terraform-digitalocean-droplet)
  - [Terraform State File](#terraform-state-file)
  - [Desired State and Current State](#desired-state-and-current-state)
  - [Provider Architecture](#provider-architecture)
  - [Types of Terraform Provides](#types-of-terraform-provides)
    - [Configure 3rd Party provider](#configure-3rd-party-provider)
- [Managing Configurations](#managing-configurations)
  - [Attributes & Outputs in Terraform](#attributes--outputs-in-terraform)
  - [Referencing Cross-Account Resource attributes](#referencing-cross-account-resource-attributes)
  - [Terraform Variables](#terraform-variables)
  - [Variable Assignment](#variable-assignment)
  - [Count and Count Index in Terraform](#count-and-count-index-in-terraform)
  - [Conditional Expression in Terraform](#conditional-expression-in-terraform)
  - [Local Values](#local-values)
  - [Terraform Functions](#terraform-functions)
  - [Data Sources in Terraform](#data-sources-in-terraform)
  - [Debugging in Terraform](#debugging-in-terraform)
  - [Formatting in Terraform](#formatting-in-terraform)
  - [Validate Terraform Files](#validate-terraform-files)
  - [Load Order & Semantics in Terraform](#load-order--semantics-in-terraform)
  - [Dynamic Blocks](#dynamic-blocks)
  - [Terraform Taint](#terraform-taint)
  - [Splat Expression](#splat-expression)
  - [Terraform Graph](#terraform-graph)
  - [Saving Terraform Plan to a file](#saving-terraform-plan-to-a-file)
  - [Terraform Output](#terraform-output)
  - [Terraform Settings](#terraform-settings)
  - [Handliing Larger Infrastructure](#handliing-larger-infrastructure)
- [Terraform Provisioners](#terraform-provisioners)
  - [Types of Provisioners](#types-of-provisioners)
    - [remote-exec Provisioners](#remote-exec-provisioners)
    - [local-exec Provisioner](#local-exec-provisioner)
- [Modules and Workspaces](#modules-and-workspaces)
  - [Understanding DRY Principle](#understanding-dry-principle)
  - [Implementing Module](#implementing-module)
  - [Varialbls and Modules](#varialbls-and-modules)
  - [Terrform Registry](#terrform-registry)
  - [Terraform Workspace](#terraform-workspace)
- [Remote State Management](#remote-state-management)
  - [Terraform and Git Integration for Team Management](#terraform-and-git-integration-for-team-management)
  - [Module Sources in Terraform](#module-sources-in-terraform)
  - [Terraform and .gitignore](#terraform-and-gitignore)
  - [Remote State Management with Terraform](#remote-state-management-with-terraform)
  - [Implementing S3 Backend](#implementing-s3-backend)
  - [Understanding State File Locking](#understanding-state-file-locking)
  - [Using DynamoDB with S3 for State Locking](#using-dynamodb-with-s3-for-state-locking)
  - [Terraform State Management](#terraform-state-management)
  - [Importing Existing Resource](#importing-existing-resource)
- [Security](#security)
  - [Multi-region and Multi-profile deployment](#multi-region-and-multi-profile-deployment)
  - [Terraform with STS](#terraform-with-sts)
  - [Handling Sensitive Data in Output](#handling-sensitive-data-in-output)
- [Terraform Cloud](#terraform-cloud)
  - [Sentinel](#sentinel)
  - [Overview of Remote Backends](#overview-of-remote-backends)
- [Appendix A - Useful References](#appendix-a---useful-references)
- [Appendix B - Notes](#appendix-b---notes)
  - [Terraform Enterprise and Terraform Cloud](#terraform-enterprise-and-terraform-cloud)
- [Appendix C - Frequently Asked Questions](#appendix-c---frequently-asked-questions)
- [Appendix D - Other Commands to Refer](#appendix-d---other-commands-to-refer)
- [Appendix E - Questions](#appendix-e---questions)

<a target="_blank" href="https://shareasale.com/r.cfm?b=1612350&amp;u=2435164&amp;m=59485&amp;urllink=&amp;afftrack="><img src="https://static.shareasale.com/image/59485/September_PromoAds_901x501.png" border="0" /></a>

# Introduction

## References to Start

- [Get Started](https://learn.hashicorp.com/terraform)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-study)
- [Exam Review - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-review)
- [Sample Questions - Terraform Associate Certification](https://learn.hashicorp.com/tutorials/terraform/associate-questions)
- [Terraform Commands (CLI)](https://www.terraform.io/docs/commands/)
- [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- [HashiCorp Infrastructure Automation Certification](https://www.hashicorp.com/certification/terraform-associate/)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/terraform/certification/terraform-associate-study-guide)

---
- [What is Mutable vs. Immutable Infrastructure?](https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure/)(hashicorp)
- [What Is Immutable Infrastructure?](https://www.digitalocean.com/community/tutorials/what-is-immutable-infrastructure)(digitalocean)


---
- [250 Practice Questions For Terraform Associate Certification](https://medium.com/bb-tutorials-and-thoughts/250-practice-questions-for-terraform-associate-certification-7a3ccebe6a1a)
- [Terraform Beginners Track](https://github.com/collabnix/terraform/blob/master/beginners/README.md) - (collabnix - GitHub)
- [Using Ansible to automate app deployment on Terraform-provided infrastructure](https://cloud.ibm.com/docs/terraform?topic=terraform-ansible)
- [Writing Ansible Playbooks for New Terraform Servers](https://victorops.com/blog/writing-ansible-playbooks-for-new-terraform-servers) - (victorops.com)
- [Terraform Course - Automate your AWS cloud infrastructure](https://www.youtube.com/watch?v=SLB_c_ayRMo) (freeCodeCamp.org)
- [HashiCorp Certified Terraform Associate - Overview](https://www.youtube.com/watch?v=vhZEdqlXlSs) (Video)
- [terraform-beginner-to-advanced-resource - GitHub](https://github.com/zealvora/terraform-beginner-to-advanced-resource)
- [Guidance on HashiCorp Certified — Terraform Associate](https://medium.com/@ravadonis/guidance-on-hashicorp-certified-terraform-associate-1fa6f04af1d2) -(Ravichandran Somasundaram on Medium)

# Creating first Instance using Terraform

[Providers](https://www.terraform.io/docs/providers/index.html)

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
  region     = "ap-southeast-1"
  access_key = "my-access-key"
  secret_key = "my-secret-key"
  version = ">=2.0"
}

resource "aws_instance" "web" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = "t2.micro"

  tags = {
    Name = "FirstEC2"
  }
}
```

Then, 
- `terraform init` which will download and configure plugins which we have mentioned in the terraform file.
- `terraform plan` will show you the items going to create
- `terraform apply` will create the resources as per terraform template.
- `-auto-approve` will not ask for confirmation

## Destroying Resource

- `terraform destoy` will delete the resources
- `terraform destroy -target aws_instance.web` - destroy specific resource only.
- also you can comment out the resource, then terraform will detect it as not part of config and will remove when you do `plan` or `apply`

## Terraform DigitalOcean Droplet
To generate API tokens from Digital Ocean

[Generate DigitalOcean Token](https://cloud.digitalocean.com/account/api/tokens)

```
provider "digitalocean" {
  token = "YOUR-TOKEN"
}

resource "digitalocean_droplet" "doinstance" {
  image  = "ubuntu-18-04-x64"
  name   = "prod"
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
   - for some proprietary platform to use with Terraform

### Configure 3rd Party provider
- Place the plugin in specific directory
  - Windows: `%APPDATA%\terraform.d\plugins`
  - All other systems: `~/.terraform.d/plugins`

# Managing Configurations

- keep configurations in directories

## Attributes & Outputs in Terraform

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

1. Environment variables - can use environment variable with a prefix `TF_VAR_`. eg: 
   
   `export TF_VAR_instance_type=t2.micro`

2. Command Line Flags
   
   `terraform plan -var="instancetype=t2.small"`

3. From a File - use `terraform.tfvars` - terraform will load all variables from this file. If different var files to be used, then
   
   `terraform plan -var-file="custom.tfvars`
   
4. Variable Defaults - can keep variable default in another `.tf` file.
  
```
$ cat variables.tf 
variable "my_ip" {
  default = "10.1.10.10/32"
}
```

- if no value mentioned, then `default` value will be used.
- if `default` value not defined, then terraform will ask for variable when do `apply` or `plan`

##Variables Datatypes

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

## Count and Count Index in Terraform

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

## Conditional Expression in Terraform

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

- Define and use inside resources

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

- You can test functions by `terraform console`
  

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

## Data Sources in Terraform

[Doc - Data Source](https://www.terraform.io/docs/configuration/data-sources.html)

- Allow data to be fetched from external sources and use inside config dynamically.

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
- `export TF_LOG=TRACE` to see details logs (`TRACE` is the most verbose one)
- `export TF_LOG_PATH=YOUR_PATH_FOR_LOG` will save logs in file. (You should set `TF_LOG`)

## Formatting in Terraform

- Use `terraform fmt` to cleanup the code

## Validate Terraform Files

- `terraform validate` - Check whether a configuration is syntactically valid or not
- check if any unsupported arguments, any undeclared variables etc

## Load Order & Semantics in Terraform

- Terraform generally loads all the config files - `.tf` & `.tf.json` - within the directory, specified in alphabetical order.
- Split the code into multiple files, eg:
  - `provider.tf`
  - `variables.tf`
  - `ec2.tf`
  - `iam.tf` 
  - etc.

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
- Use `iterator` for better reading (eg: `iterator = port` above)

## Terraform Taint

- manually marks a terraform managed resource as trainted and forcing it to be destroyed and recreated on the next apply.
- `terraform taint` command will make modification in the `tfstate` file and recreate action will happen in next apply.
- `terraform taint` command will not modify the `.tf` file or infrastructure.

```
terraform taint aws_instance.myec2
```

## Splat Expression

- use wildcards to get multiple resource information. 

```
# single resource
output "arn-single" {
  value = aws_iam_user.lb[0].arn
}

# multiple resource
output "arns" {
  value = aws_iam_user.lb[*].arn
}
```

## Terraform Graph

- Generate visual representation of either a configuration or execution plan.
  
```
terraform graph > graph.dot
```

- DOT format and can convert to an image (Use 3rd party tool)

```
sudo apt-get install graphviz
# or 
sudo yum install graphviz

# then
cat graph.dot | dot -Tsvg > graph.svg
```

## Saving Terraform Plan to a file

- Save terraform plan content for later use - `terraform plan -out=backpup-01` (will be a binary file)
- Later use this file to create resource - `terraform apply backpup-01` 

## Terraform Output

- To display or extract the value of an output variable from the state file.

```
terraform output iam_names
```

## Terraform Settings
- To configure the behavior of terraform itself we can use `terraform` block
- `required_providers` - specifies all of the providers needed for the modules, mapping each local provider names to a source address and a version constraints.

```
terraform {
  required_version = "> 0.12.0"

  required_providers {
    aws = "~> 2.0"
    mycloud = {
      source = "mycloud/mycloud"
      version = "~> 1.0"
    }
  }
}
```
## Handliing Larger Infrastructure
- always split into smaller files and explicitly call
- use `-refresh=false` to prevent terraform from querying the current state during operations like `terraform plan`
- use `-target=resource` flag to do terraform action only on that resource

# Terraform Provisioners

- Can be used to model specific actions on the local machine or on a remote machine in order to prepare servers or other infra objects for service.

Ref: 
- [Provisioners](https://www.terraform.io/docs/provisioners/index.html)
- [Provision Infrastructure with Packer](https://learn.hashicorp.com/tutorials/terraform/packer)

## Types of Provisioners

**Types**

1. Creation-Time Provisiones
  - if creation time provisioner fails, the resource will be marked as tainted.
  
2. Destroy-Time Provisioners
  - run only when there is `when = destroy` option mentioned.

**Failure Behaviours Options**

- `on_failure = continue` - Ignore and continue with creation or destroy action
- `on_failure = fail` - Stop applying and show error; also taint the resource if this is a creation task

### remote-exec Provisioners

Ref: [Provisioner Connection Settings](https://www.terraform.io/docs/provisioners/connection.html)

- invoke actions on remote machine which terraform created

```
resource "aws_instance" "myec2" {
   ami = "ami-082b5a644766e0e6f"
   instance_type = "t2.micro"
   key_name = "kplabs-terraform"

   provisioner "remote-exec" {
     inline = [
       "sudo amazon-linux-extras install -y nginx1.12",
       "sudo systemctl start nginx"
     ]

   connection {
     type = "ssh"
     user = "ec2-user"
     private_key = file("./kplabs-terraform.pem")
     host = self.public_ip
   }
   }
}
```

- A `connection` block nested directly within a resource affects all of that resource's provisioners.
- A `connection` block nested in a provisioner block only affects that provisioner, and overrides any resource-level connection settings.

### local-exec Provisioner

[Doc](https://www.terraform.io/docs/provisioners/local-exec.html)

- invoke local executable after resource is created
- can execute ansible playbooks on the created server
  
```
resource "aws_instance" "myec2" {
   ami = "ami-082b5a644766e0e6f"
   instance_type = "t2.micro"

   provisioner "local-exec" {
    command = "echo ${aws_instance.myec2.private_ip} >> private_ips.txt"
  }
}
```

Another example:
```
provisioner "local-exec" {
  command = "sleep 120; ansible-playbook -i '${digitalocean_droplet.www-example.ipv4_address}' playbook.yml"
}
```

# Modules and Workspaces

## Understanding DRY Principle

- **Dont Repeat Yourself*
- redution repetition of software patters
- define in source and refer it in resources

## Implementing Module

[Doc](https://www.terraform.io/docs/configuration/modules.html)

Define the structure in modile dir.

`project_dir/modules/ec2` : 

```
resource "aws_instance" "myec2" {
   ami = "ami-082b5a644766e0e6f"
   instance_type = var.instance_type
}
```

and in project just reference the same

`project_dir/a-project/` : 
```
module "ec2module" {
  source = "../../modules/ec2"
}
```

Dir structure :
```
$ tree ../module-demo/
../module-demo/
├── a-project
│   ├── myec2.tf
│   └── provider.tf
├── b-project
│   ├── myec2.tf
│   └── provider.tf
└── modules
    └── ec2
        └── module-ec2.tf

4 directories, 5 files
```

## Varialbls and Modules

- you cannot change argument values while calling the module as the values are hardcoded in module; instead use variable in module

```
resource "aws_instance" "myec2" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = var.instance_type
}
```

- and the variable need to define in module directory - `variables.tf`, with default values.

```
variable "instance_type" {
  default = "t2.micro"
}
```

- and from project, you can overwrite the variable as needed while calling the module

```
module "ec2module" {
  source        = "../modules/ec2"
  instance_type = "t2.large"
}
```

## Terrform Registry

[Terraform Registry](https://registry.terraform.io/)

[Doc](https://www.terraform.io/docs/cloud/registry/using.html)

- The public registry uses a three-part <NAMESPACE>/<MODULE_NAME>/<PROVIDER> format,
- Private modules use a four-part <HOSTNAME>/<ORGANIZATION>/<MODULE_NAME>/<PROVIDER> format.

Notes :
- repository of modules writtern by the Terraform community
- you can use available modules from registry instead of writing your own.
- verified modules which are maintained by 3rd party vendors are also available in Terraform Registry
- a blue verification badge appears next to the module
- Search in `registry.terraform.io` and filter with verified modules.
- Basic usage samples, variables etc will be displayed in the module page
- You can reference to the module directly from Terraform registry
- mention the module version as needed

Ref: [ec2-instance](https://registry.terraform.io/modules/terraform-aws-modules/ec2-instance/aws/2.15.0)

```
module "ec2_cluster" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 2.0"

  name                   = "my-cluster"
  instance_count         = 5

  ami                    = "ami-ebd02392"
  instance_type          = "t2.micro"
  key_name               = "user1"
  monitoring             = true
  vpc_security_group_ids = ["sg-12345678"]
  subnet_id              = "subnet-eddcdzz4"

  tags = {
    Terraform   = "true"
    Environment = "dev"
  }
}
```

- When you do `terraform init`, terraform will download the module to local path `./.terraform/modules/`

## Terraform Workspace

[Workspaces](https://www.terraform.io/docs/state/workspaces.html)

- different workspace to manage environment, eg; different variables.

```
$ terraform workspace show
default

$ terraform workspace new dev
Created and switched to workspace "dev"!

You're now on a new, empty workspace. Workspaces isolate their state,
so if you run "terraform plan" Terraform will not see any existing state
for this configuration.

$ terraform workspace list
  default
* dev

$ terraform workspace select dev
Switched to workspace "dev".
```

Sample usage:
```
provider "aws" {
  region     = "ap-southeast-1"
  shared_credentials_file = "$HOME/.aws/credentials"
  profile                 = "default"
  version = ">=2.0"
}

resource "aws_instance" "myec2" {
  ami           = "ami-0cd31be676780afa7"
  instance_type = lookup(var.instance_type,terraform.workspace)

}

variable "instance_type" {
 type = map

 default = {
   default = "t2.nano"
   stage = "t2.nano"
   dev = "t2.micro"
   prod = "t2.large"
 }
}
```
- terraform will create separate `terraform.tfstate` files in `terraform.tfstate.d/WORKSPACE_NAME/` directories in the project directoy

```
$ tree terraform.tfstate.d/
terraform.tfstate.d/
├── dev
│   └── terraform.tfstate
├── prod
└── stage
    └── terraform.tfstate

3 directories, 2 files
```

# Remote State Management

[Remote State](https://www.terraform.io/docs/state/remote.html)

## Terraform and Git Integration for Team Management 

- never keep credentials or secrets in repo; use safer methods
eg: `password = "${file(../db_password.txt)}"` 
- **DO NOT** store `terraform.tfstate` file in repo as it will store credentials or secrets in plain text

## Module Sources in Terraform

**Supported Module Sources**
- Local Path - Must be `./` or `../` to indicate the local path is intended
  
```
module "ec2module" {
  source = "../../modules/ec2"
}
```

- Terraform Registry
- GitHub, BitBucket, Generic Git, Mercurial Repos

```
module "vpc" {
  source = "git::https://example.com/vpc.git"
}

module "storage" {
  source = "git::ssh://username@example.com/storage.git"
}

module "vpc" {
  source = "git::https://example.com/vpc.git?ref=v1.2.0"
}

module "mymodule" {
  source = "github.com/techbeatly/module-repo"
}
```

- HTTP URLs
- S3 buckets
- GCS Buckets

## Terraform and .gitignore

Files to ignore
- `.terraform` 
- `terraform.tfvars` - sensitive data like username or password
- `terraform.tfstate` - should be stored in the remote side
- `crash.log`

Sample `.gitignore`

```
# Local .terraform directories
**/.terraform/*

# .tfstate files
*.tfstate
*.tfstate.*

# Crash log files
crash.log

# Exclude all .tfvars files, which are likely to contain sentitive data, such as
# password, private keys, and other secrets. These should not be part of version 
# control as they are data points which are potentially sensitive and subject 
# to change depending on the environment.
#
*.tfvars

# Ignore override files as they are usually used to override resources locally and so
# are not checked in
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Include override files you do wish to add to version control using negated pattern
#
# !example_override.tf

# Include tfplan files to ignore the plan output of command: terraform plan -out=tfplan
# example: *tfplan*

# Ignore CLI configuration files
.terraformrc
terraform.rc
```

## Remote State Management with Terraform

- Store tfstate in remote location securly
- Multiple options supported by terraform
  - Standard Backend - State Storage and Locking
  - Enhanced Backend - All features of Standard + Remote Management

## Implementing S3 Backend

[Doc](https://www.terraform.io/docs/backends/types/s3.html)

- create an s2 bucket manually and create `backend` to store the tfstate
  
```
terraform {
  backend "s3" {
    bucket = "terraform-remote-demo"
    key    = "remote-terraform-state-demo.tfstate"
    region = "ap-southeast-1"
  }
}
```

## Understanding State File Locking
- consider issue when multiple people working on the tarraform config parallel writing on `tfstate` file.
- terraform will lock the `tfstate` file.
- need to mention the locking in backend (S3 by default not support any locking)

## Using DynamoDB with S3 for State Locking

- create a DynamoDB table for storing lock details
Table Name : <your-table-name>
Primay Key : <LockID>

- and update the `backend` with `dynamodb_table` entry

```
terraform {
  backend "s3" {
    bucket = "terraform-remote-demo"
    key    = "remote-terraform-state-demo.tfstate"
    region = "ap-southeast-1"
    dynamodb_table = "tf-state-demo"
  }
}
```
- now, when you run `terraform plan` or `apply`, you will see an entry in dynamodb table but only until the command finish. This will allow terraform to avoid multiple actions to be run on same tfstate file.

## Terraform State Management

- `terraform state` - for advanced state management

```
Subcommands:
    list                List resources in the state
    mv                  Move an item in the state
    pull                Pull current state and output to stdout
    push                Update remote state from a local state file
    replace-provider    Replace provider in the state
    rm                  Remove instances from the state 
                        but it will not remove the resouce from cloud/provider
    show                Show a resource in the state
```

Eg:


- if you rename or change something on a resource, terraform will destroy and recreate the resource with new name. To avoud this, you can use `terraform state move`

```
$ terraform state list
aws_iam_user.lb
aws_instance.myec2

$ terraform state mv aws_instance.myec2 aws_instance.myec2new
Move "aws_instance.myec2" to "aws_instance.myec2new"
Successfully moved 1 object(s).

$ terraform state list
aws_iam_user.lb
aws_instance.myec2new
```

- `terraform state rm` will remove the resource from state but it will not remove the resouce from cloud/provider.
```
$ terraform state rm aws_instance.myec2
Removed aws_instance.myec2
Successfully removed 1 resource instance(s).
```

- but next time when you run `terraform plan` or `apply`, terraform will recreate the instance as again as the resource definition is still there.
- `terraform state show RESOURCE_NAME` will show details of a specific resource

```
$ terraform state show aws_instance.myec2
``` 

## Importing Existing Resource 
- importe resources which are created manually
- create the `.tf` file based on existing resource

```
resource "aws_instance" "myec2" {
  ami                   = "ami-0b1e534a4ff9019e0"
  instance_type         = "t2.micro"
  vpc_security_group_id = ["vpc-4a59ba2c"]
  key_name              = "tf-20200805"
  subnet_id             = "subnet-3f9f5877"

  tags {
    Name = "test"
  }
}
```

- now import the resource to state

```
$ terraform import aws_instance.myec2 i-034503eb8b60d3a51
aws_instance.myec2: Importing from ID "i-034503eb8b60d3a51"...
aws_instance.myec2: Import prepared!
  Prepared aws_instance for import
aws_instance.myec2: Refreshing state... [id=i-034503eb8b60d3a51]

Import successful!

The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.
```

# Security

- keep provider credential out of project place (like use aws cli etc)

## Multi-region and Multi-profile deployment

- define multiple provider blocks with alias and refer provider in resource block
- also add separate profile in provider block

```
provider "aws" {
  region     =  "us-west-1"
}

provider "aws" {
  alias      =  "aws02"
  region     =  "ap-south-1"
  profile    =  "account02"
}
```

and in resources, call proper providers.
```
resource "aws_eip" "myeip" {
  vpc = "true"
}

resource "aws_eip" "myeip01" {
  vpc = "true"
  provider = "aws.aws02"
}
```

## Terraform with STS

```
provider "aws" {
  region                  = "ap-southeast-1"
  assume_role {
    role_arn = "YOUR_ROLE_ARN"
    session_name = "sts-arn-demo"
  }
}
```

## Handling Sensitive Data in Output

```
locals {
  db_password = {
    admin = "password"
  }
}

output "db_password" {
  value = local.db_password
  sensitive   = true
}
```

# Terraform Cloud

GUI

## Sentinel

[Doc](https://docs.hashicorp.com/sentinel/terraform/)

- embeded policy-as-code framework
- paid feature

```
import "tfplan"
 
main = rule {
  all tfplan.resources.aws_instance as _, instances {
    all instances as _, r {
      (length(r.applied.tags) else 0) > 0
    }
  }
}
```

## Overview of Remote Backends

[Doc](https://www.terraform.io/docs/backends/types/remote.html)

- Terraform operations can execute in Terraform cloud and can see the output in local terminal
```
terraform {
  #required_version = "~> 0.12.0"
  backend "remote" {}
}

resource "aws_iam_user" "lb" {
  name = "remoteuser"
  path = "/system/"
}
```

and, we have `backend.hcl`
```
workspaces { name = "terraform-iac-usecases" }
hostname     = "app.terraform.io"
organization = "techbeatly"
```

Then, `terraform login` which will ask for token from [API](https://app.terraform.io/app/settings/tokens) and will show the success message.

Then, `terraform init` with `backend`

```
$ terraform init -backend-config=backend.hcl

$ terraform apply
```

The same settings can alternatively be specified on the command line as follows:
```
$ terraform init \
    -backend-config="address=demo.consul.io" \
    -backend-config="path=example_app/terraform_state" \
    -backend-config="scheme=https"
```

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

Primary Benefits of Infrastructure as Code
- Automation
- Versioning
- Reusability

## Terraform Enterprise and Terraform Cloud
- SSO
- Auditing
- Private Data Center Networking
- Clustering



# Appendix C - Frequently Asked Questions

**Q. Is it mandatory to keep the variables in `variables.tf` ?**

# Appendix D - Other Commands to Refer

Unlock terraform state
```
terraform force-unlock LOCK_ID [DIR]
```

Upgrade the provider version to the latest acceptable one. 
```
terraform init -upgrade
```

# Appendix E - Questions


<a target="_blank" href="https://shareasale.com/r.cfm?b=1612350&amp;u=2435164&amp;m=59485&amp;urllink=&amp;afftrack="><img src="https://static.shareasale.com/image/59485/September_PromoAds_901x501.png" border="0" /></a>


