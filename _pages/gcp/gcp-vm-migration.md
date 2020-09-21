---
layout: post
title: Ansible for VMware
categories: [ cloud ]
tags: []
show-avatar: false
permalink: gcp-vm-migration
featured: false
hidden: false
showindex: true
---

## VM Migration: Planning


terraform -v

git clone https://github.com/morgante/terraform-codelab.git
cd terraform-codelab/lab-networking

edit terraform.tfvars
 -> open editor
  project_id="qwiklabs-gcp-01-eb85e88d83fc"


gcloud iam service-accounts create terraform --display-name terraform

gcloud iam service-accounts list

gcloud iam service-accounts keys create ./credentials.json --iam-account terraform@qwiklabs-gcp-01-eb85e88d83fc.iam.gserviceaccount.com

gcloud projects add-iam-policy-binding qwiklabs-gcp-01-eb85e88d83fc --member=serviceAccount:terraform@qwiklabs-gcp-01-eb85e88d83fc.iam.gserviceaccount.com --role=roles/owner


--- setup remote state

gsutil mb gs://qwiklabs-gcp-01-eb85e88d83fc-state-bucket

-- edit backend.tf

terraform {
  backend "gcs" {
    bucket = "qwiklabs-gcp-01-eb85e88d83fc-state-bucket"       # Change this to <my project id>-state-bucket
    prefix = "terraform/lab/network"
  }
}


--- run terraform

terraform init

terraform plan

terraform apply

terraform show

-- add a subnet

edit network.tf

```
# # Create the network
module "vpc" {
  source  = "terraform-google-modules/network/google"
  version = "~> 0.4.0"

  # Give the network a name and project
  project_id   = "${google_project_service.compute.project}"
  network_name = "my-custom-network"

  subnets = [
    {
      # Creates your first subnet in us-west1 and defines a range for it
      subnet_name   = "my-first-subnet"
      subnet_ip     = "10.10.10.0/24"
      subnet_region = "us-west1"
    },
    {
      # Creates a dedicated subnet for GKE
      subnet_name   = "my-gke-subnet"
      subnet_ip     = "10.10.20.0/24"
      subnet_region = "us-west1"
    },
    # Add your subnet here
  ]

  # Define secondary ranges for each of your subnets
  secondary_ranges = {
    my-first-subnet = []

    my-gke-subnet = [
      {
        # Define a secondary range for Kubernetes pods to use
        range_name    = "my-gke-pods-range"
        ip_cidr_range = "192.168.64.0/24"
      },
    ]
    # Add your subnet’s secondary range below this line.

  }
}

```


terraform apply
