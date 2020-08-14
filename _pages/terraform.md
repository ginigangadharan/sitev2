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
---

# References to Start

- [Get Started](https://learn.hashicorp.com/terraform)
- [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- [Terraform Beginners Track](https://github.com/collabnix/terraform/blob/master/beginners/README.md) - (collabnix - GitHub)
- [Using Ansible to automate app deployment on Terraform-provided infrastructure](https://cloud.ibm.com/docs/terraform?topic=terraform-ansible)
- [Writing Ansible Playbooks for New Terraform Servers](https://victorops.com/blog/writing-ansible-playbooks-for-new-terraform-servers) - (victorops.com)
- [Terraform Course - Automate your AWS cloud infrastructure](https://www.youtube.com/watch?v=SLB_c_ayRMo) (freeCodeCamp.org)
- [HashiCorp Infrastructure Automation Certification](https://www.hashicorp.com/certification/terraform-associate/)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/terraform/certification/terraform-associate-study-guide)
- [HashiCorp Certified Terraform Associate - Overview](https://www.youtube.com/watch?v=vhZEdqlXlSs) (Video)
  
**Disclaimer: Scrap notes only, DO NOT refer for production !!!**

```
provisioner "local-exec" {
  command = "sleep 120; ansible-playbook -i '${digitalocean_droplet.www-example.ipv4_address}' playbook.yml"
}
```