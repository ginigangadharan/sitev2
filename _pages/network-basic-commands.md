---
layout: post
title: Basic Network Device Commands and Setup
categories: [ Network, Automation ]
tags: []
show-avatar: false
permalink: network-basic-commands
featured: false
hidden: false
---

These are very basic network commands and setup instructions for my Ansible Network Automation labs. I am keeping those here for quick reference in case I am in trouble with a quick lab setup in GNS3.

<!-- TOC -->

- [](#)
- [Enable SSH on HPE Comware7 Devices](#enable-ssh-on-hpe-comware7-devices)
- [Assign IP Address](#assign-ip-address)
- [Configure SSH/tty and login on Cisco Devices](#configure-sshtty-and-login-on-cisco-devices)
- [Add a user for remote access](#add-a-user-for-remote-access)
- [Configure Router interface](#configure-router-interface)
- [Configure routes](#configure-routes)

<!-- /TOC -->

# Enable SSH on HPE Comware7 Devices

# Assign IP Address

# Enable SSH/tty and login on Cisco Devices

```
(config)# line vty 0 4
(config-line)# transport input all
(config-line)# login local

(config)# ip domain-name mydomain.com
(config)# crypto key generate rsa

# enable ssh v2
(config)# ip ssh version 2 
```

# Add a user for remote access

```
configure terminal
username user1 password mypassword
                                # create a user with password
username user1 privilege 15 
                                # configure privilege     
enable password mypassword
                                # setting enable password                         
### Sample
username cisco password C!sco123

username cisco privilege 15
#or 
username cisco role priv-15

enable password C!sco123
#or
feature privilege
enable secret 0 admin priv-lvl 15
```

# Configure Router interface
```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int f0/0                           
R1(config-if)#ip address 10.1.1.100 255.255.255.0

R1(config-if)#int loop 0                         
R1(config-if)#ip address 1.1.1.1 255.255.255.0   

R1(config)#int f0/0
R1(config-if)#no shut
```

# Configure routes
```
R1(config)#router ospf 1
R1(config-router)#network 0.0.0.0 255.255.255.255 area 0

R2#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/DR         00:00:34    10.1.1.101      FastEthernet0/0

R2#sh ip route
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is not set

     1.0.0.0/32 is subnetted, 1 subnets
O       1.1.1.1 [110/11] via 10.1.1.101, 00:01:06, FastEthernet0/0
     2.0.0.0/32 is subnetted, 1 subnets
C       2.2.2.2 is directly connected, Loopback0
     10.0.0.0/24 is subnetted, 1 subnets
C       10.1.1.0 is directly connected, FastEthernet0/0

```

## Enabled Terminal monitoring

```
# ter mon
```

