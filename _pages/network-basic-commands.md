---
layout: post
title: Basic Network Device Commands and Setup
categories: [ Network, Automation ]
tags: []
show-avatar: false
permalink: network-basic-commands
featured: false
hidden: false
titleshort: Network
---

These are very basic network commands and setup instructions for the Ansible Network Automation labs. I am keeping those here for quick reference in case I am in trouble with a quick lab setup in GNS3. Use at your own risk (!)

- [Cisco Set Password](#cisco-set-password)
- [Add a user for remote access](#add-a-user-for-remote-access)
- [Cisco NX9000 Initial Config](#cisco-nx9000-initial-config)
- [Cisco NXOS vrf](#cisco-nxos-vrf)
- [Cisco Set IP Address](#cisco-set-ip-address)
- [Cisco ASA Name interface and change security level](#cisco-asa-name-interface-and-change-security-level)
- [Cisco General Commands](#cisco-general-commands)
- [Get DHCP IP for VPC Node](#get-dhcp-ip-for-vpc-node)
- [Enable SSH on HPE Comware7 Devices](#enable-ssh-on-hpe-comware7-devices)
- [Assign IP Address](#assign-ip-address)
- [Enable SSH/tty and login on Cisco Devices](#enable-sshtty-and-login-on-cisco-devices)
- [Setting Up SSH and Local Authentication on Cisco ASA](#setting-up-ssh-and-local-authentication-on-cisco-asa)
- [Cisco ASA check Ciphers](#cisco-asa-check-ciphers)
- [Configure routes](#configure-routes)
- [SSH Ciphers](#ssh-ciphers)
- [Enabled Terminal monitoring](#enabled-terminal-monitoring)
- [Enable Clipboard copy in XTerm](#enable-clipboard-copy-in-xterm)

## Cisco Set Password

```shell
conf t
## change admin password
username admin password <securepassword> priv 15

## To change the enable password
enable password <securepassword>
```

## Add a user for remote access

```shell
configure terminal
username user1 password mypassword
                                # create a user with password
username user1 privilege 15 
                                # configure privilege     
enable password mypassword
                                # setting enable password                         
## Sample
username cisco password C!sco123

username cisco privilege 15
## or 
username cisco role priv-15

enable password C!sco123
## or
feature privilege
enable secret 0 admin priv-lvl 15
```

## Cisco NX9000 Initial Config

```shell
The following configuration will be applied:
  password strength-check
  switchname NX9K
vrf context management
ip route 0.0.0.0/0 192.168.57.1
exit
  feature telnet
  ssh key rsa 1024 force
  feature ssh
  system default switchport
  no system default switchport shutdown
  copp profile strict
interface mgmt0
ip address 192.168.57.122 255.255.255.0
no shutdown
```

## Cisco NXOS vrf

```shell
NX9K(config)# ping 192.168.57.106 vrf management
PING 192.168.57.106 (192.168.57.106): 56 data bytes
64 bytes from 192.168.57.106: icmp_seq=0 ttl=63 time=2.202 ms
64 bytes from 192.168.57.106: icmp_seq=1 ttl=63 time=3.035 ms
64 bytes from 192.168.57.106: icmp_seq=2 ttl=63 time=2.545 ms
64 bytes from 192.168.57.106: icmp_seq=3 ttl=63 time=3.184 ms
64 bytes from 192.168.57.106: icmp_seq=4 ttl=63 time=4.323 ms

--- 192.168.57.106 ping statistics ---
5 packets transmitted, 5 packets received, 0.00% packet loss

## Copy to tftp
NX9K(config)# copy running-config tftp://192.168.57.106/test-con vrf management
Trying to connect to tftp server......
Connection to Server Established.
                                                                          TFTP put operation was successful
Copy complete, now saving to disk (please wait)...
Copy complete.
```

## Cisco Set IP Address

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

[Cisco ASA Series General Operations CLI Configuration Guide, 9.8](https://www.cisco.com/c/en/us/td/docs/security/asa/asa98/configuration/general/asa-98-general-config/interface-basic.html)

## Cisco ASA Name interface and change security level

```shell
## The nameif command is used to specify a name for the interface
nameif inside

## Change security level
security 90
```

## Cisco General Commands

```shell
sh ver                             # show version
sh int ip br                       # show interface IP details
```

## Get DHCP IP for VPC Node 

```shell
PC1> ip dhcp
DORA IP 192.168.122.205/24 GW 192.168.122.1
```

## Enable SSH on HPE Comware7 Devices

## Assign IP Address

## Enable SSH/tty and login on Cisco Devices

```
(config)# line vty 0 4
(config-line)# transport input all
(config-line)# login local

(config)# ip domain-name mydomain.com
(config)# crypto key generate rsa

# enable ssh v2
(config)# ip ssh version 2 
```

## Setting Up SSH and Local Authentication on Cisco ASA

Check [blog](https://www.pei.com/set-up-ssh-cisco-asa/)

```shell
## Step 1: Configure aaa to use local database for ssh and console
ciscoasa# aaa authentication ssh console LOCAL

## Step 2: Create admin username with privilege 15 (username, P@ssw0rd)
ciscoasa# username username password P@ssw0rd priv 15

## Step 3: Turn on password for enable
ciscoasa# aaa authentication enable console LOCAL

## Step 4: Turn on serial console authentication
ciscoasa# aaa authentication serial console LOCAL

## Step 5: Save the changes so far
ciscoasa# write mem

## Step 6: log out console and verify access
ciscoasa(config)# end
ciscoasa# exit
Logoff
Username: username
Password: ********

## Step 7: Generate ssh key pair
ciscoasa# crypto key generate rsa modulus 4096
INFO: The name for the keys will be: <Default-RSA-Key>
Keypair generation process begin. Please wait…
ciscoasa(config)#***NOTE*** SSH is an encrypted protocol, uses RSA to generate public and private key
4096 = block size
rsa = encryption algorithm
Step 8: Allow access to the inside interface
ciscoasa# ssh 0.0.0.0 0.0.0.0 inside

## Step 9: Force ssh version 2
ciscoasa# ssh version 2

## Step 10: Add timeout of 15 min to ssh
ciscoasa# ssh timeout 15
```

## Cisco ASA check Ciphers

```shell
show ssl ciphers all

## Change the group1-sha1 to group14-sha1 in Cisco ASA 5506X (9.8(2))
## Use only SSH v2 and change to use dh-group14-sha1

ssh version 2
ssh key-exchange group dh-group14-sha1

## login to ASA which offer only diffie-hellman-group1-sha1
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -l <username> <ip_address>
```

- [Cisco ASA Firewall – Diffie-Hellman-Group1-Sha1](https://aventistech.com/2018/09/19/cisco-asa-firewall-diffie-hellman-group1-sha1/)

## Configure routes

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

## SSH Ciphers

```shell
ssh -Q cipher       # List supported ciphers
ssh -Q mac          # List supported MACs
ssh -Q key          # List supported public key types
ssh -Q kex          # List supported key exchange algorithms
```

## Enabled Terminal monitoring

```
ter mon
```

## Enable Clipboard copy in XTerm

```shell
## Add to the file ~/.Xresources (or create):
XTerm*selectToClipboard: true

## Then run the command:
xrdb -merge ~/.Xresources

## Restart xterm.
```