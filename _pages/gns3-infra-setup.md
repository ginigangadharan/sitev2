---
layout: post
title: GNS3 Infra Setup
author: gini
categories: [ ansible ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: [network lab, network automation, ansible network automation]
permalink: /gns3-infra-setup
featured: false
hidden: false
---

Step 1
- setup virtualbox or VMWare ESXi node

Step 2
- [Download](https://github.com/GNS3/gns3-gui/releases) GNS3 image (OVA or suitable format)

Step 3
- Create a VM in ESXi (or in VirtualBox) with downloaded ova file
- configure IP as you need

Step 4
- Download and install GNS3 GUI on a machine (your workstation or laptop) https://www.gns3.com/
- Refer install instruction - [linux](https://docs.gns3.com/1QXVIihk7dsOL7Xr7Bmz4zRzTsJ02wklfImGuHwTlaA4/index.html)

Step 5
- Open GNS3 GUI -> Connect to GNS3 VM/Image

Step 6
- Create Project
- 

Step 7
- Download ISO images for devices 
- or IOS



Connect to real network
- [Connecting GNS3 to Real Network in Windows 10 Home](https://www.gns3.com/qa/connecting-gns3-to-real-network-)
- [GNS3 Talks: How to connect GNS3 to a physical network (Part 1).](https://www.youtube.com/watch?v=Gpje9PV1j8U)
- [Connect GNS3 to the Internet (local server)](https://docs.gns3.com/1vFs-KENh2uUFfb47Q2oeSersmEK4WahzWX-HrMIMd00/index.html)

## install gns3-server locally
```
sudo apt install  gns3-server
```
