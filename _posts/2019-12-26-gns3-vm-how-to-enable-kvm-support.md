---
layout: post
title: GNS3 VM – How to Enable KVM Support
author: gini
categories: [ network, automation ]
image: "assets/images/gns3-vm-how-to-enable-kvm-support-100.png"
tags: [ansible, gns3, network automation]
ext_url: https://www.techbeatly.com/2019/12/gns3-vm-how-to-enable-kvm-support.html
#show-avatar: false
featured: false
hidden: false
---

GNS3 (Graphical Network Simulator) is the most popular – open source, free software – tool for designing and simulating network infrastructure.

GNS3 has 2 parts

- The GNS3-all-in-one software (GUI)
- The GNS3 virtual machine (VM)

This post is not meant for explaining GNS3; read [GNS3 Documentation](https://docs.gns3.com/) for more details. I just want to share the common mistake, which newbies do when setup GNS3. And the simple configuration to resolve the same.

While setting up separate GNS3 VM, you need to enable KVM support for the GNS3 VM (even if you are using all in one). Because, most of the appliance need KVM support on GNS3 VM otherwise you won’t be able to import network appliance to GNS3.

**See [Full Article](https://www.techbeatly.com/2019/12/gns3-vm-how-to-enable-kvm-support.html/) in [techbeatly.com](https://www.techbeatly.com/2019/12/gns3-vm-how-to-enable-kvm-support.html/)**
