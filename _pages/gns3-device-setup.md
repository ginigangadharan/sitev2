---
layout: post
title: Configure Devices in GNS3
author: gini
categories: [ GNS3 ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: []
show-avatar: false
permalink: /gns3-device-setup
featured: false
hidden: false
---


# Configure Cisco Device in GNS3

## References

- **Nexus 7K/N7K - [Cisco NX-OSv appliance](https://docs.gns3.com/appliances/cisco-nxosv.html)**
  
# Configure Fortigate Device in GNS3

## Download Fortios VM for GNS3
- https://support.fortinet.com/
- Login
- https://support.fortinet.com/Download/VMImages.aspx
- Download VM Zip -> Extract and get fortios.qcow2
- in GNS3 -> Preferences -> QEMU -> Qemu VMs -> Add New (give 1024MB)
- username : admin , password : <blank>
  
## Configure Fortios Device

```
FortiGate-VM64-KVM login: admin
Password: 
You are forced to change your password. Please input a new password.
New Password: 
Confirm Password: 

Welcome!

FortiGate-VM64-KVM # 
FortiGate-VM64-KVM # show system interface
FortiGate-VM64-KVM # config system interface 
FortiGate-VM64-KVM (interface) # edit port1
FortiGate-VM64-KVM (port1) # set mode static 
FortiGate-VM64-KVM (port1) # set ip 10.1.10.70 255.255.255.0
FortiGate-VM64-KVM (port1) # append allowaccess http
FortiGate-VM64-KVM (port1) # append allowaccess ssh
FortiGate-VM64-KVM (port1) # end

FortiGate-VM64-KVM # config system global
FortiGate-VM64-KVM (global) # set hostname GNS3-FortiGate
FortiGate-VM64-KVM (global) # end
```

## GNS3 References for Fortinet
- https://www.linkedin.com/pulse/fortigate-inside-gns3-cli-web-access-issa-itani/
- https://support.fortinet.com/Download/VMImages.aspx
- https://www.gns3network.com/how-to-deploy-fortigate-virtual-firewall-in-gns3/
- https://www.fortinetguru.com/2017/01/fortigate-vm-initial-configuration/