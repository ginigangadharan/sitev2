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
**Warning: Scrap notes only, DO NOT refer for production !!!**

https://github.com/collabnix/terraform/blob/master/beginners/README.md

https://www.youtube.com/watch?v=SLB_c_ayRMo&feature=youtu.be

https://cloud.ibm.com/docs/terraform?topic=terraform-ansible

https://victorops.com/blog/writing-ansible-playbooks-for-new-terraform-servers

```
provisioner "local-exec" {
  command = "sleep 120; ansible-playbook -i '${digitalocean_droplet.www-example.ipv4_address}' playbook.yml"
}
```

- [HashiCorp Infrastructure Automation Certification](https://www.hashicorp.com/certification/terraform-associate/)
- [Study Guide - Terraform Associate Certification](https://learn.hashicorp.com/terraform/certification/terraform-associate-study-guide)
- [HashiCorp Certified Terraform Associate - Overview](https://www.youtube.com/watch?v=vhZEdqlXlSs) (Video)