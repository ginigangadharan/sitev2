---
layout: post
title: Configure FortiGate in GNS3
author: gini
categories: [ GNS3 ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: /gns3-fortigate
featured: false
hidden: false
---

# Reference
- https://www.linkedin.com/pulse/fortigate-inside-gns3-cli-web-access-issa-itani/
- https://support.fortinet.com/Download/VMImages.aspx
- https://www.gns3network.com/how-to-deploy-fortigate-virtual-firewall-in-gns3/


```
FortiGate-VM64-KVM login: admin
Password: 
You are forced to change your password. Please input a new password.
New Password: 
Confirm Password: 

Welcome!

FortiGate-VM64-KVM # 
FortiGate-VM64-KVM # config system interface

FortiGate-VM64-KVM (interface) # edit port1

FortiGate-VM64-KVM (port1) # set mode static

FortiGate-VM64-KVM (port1) # set ip 10.1.10.70 255.255.255.0

FortiGate-VM64-KVM (port1) # set allowaccess https http ping ssh

FortiGate-VM64-KVM (port1) # end

FortiGate-VM64-KVM # config system global

FortiGate-VM64-KVM (global) # set hostname GNS3-FortiGate

FortiGate-VM64-KVM (global) # end

```