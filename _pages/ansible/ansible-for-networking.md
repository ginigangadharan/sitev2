---
layout: post
title: Ansible for Network Automation
categories: [ cloud ]
tags: []
show-avatar: false
permalink: ansible-for-networking
featured: false
hidden: false
---

<!-- TOC -->

- [Ansible for Network Automation](#ansible-for-network-automation)
  - [First Lab to test GNS3 setup](#first-lab-to-test-gns3-setup)
  - [Importing image to GNS3](#importing-image-to-gns3)
  - [Importing appliance to GNS3](#importing-appliance-to-gns3)
  - [Configure a router](#configure-a-router)
  - [NetworkAutomation Component](#networkautomation-component)
  - [Ansible Network Modules and Adhoc commands](#ansible-network-modules-and-adhoc-commands)
- [Ansible Ad Hoc commands:](#ansible-ad-hoc-commands)
- [Ansible CLI Playbook](#ansible-cli-playbook)
- [Appendix](#appendix)
  - [Driver is probably stuck stopping/starting](#driver-is-probably-stuck-stoppingstarting)
  - [vboxmanage cli](#vboxmanage-cli)
  - [Instal vmware workstation ubuntu](#instal-vmware-workstation-ubuntu)
  - [Add IP to VM](#add-ip-to-vm)
    - [To configure a dynamic or Static IP address](#to-configure-a-dynamic-or-static-ip-address)
    - [Configure VLAN with IP](#configure-vlan-with-ip)
    - [VLANs](#vlans)
    - [assign to port](#assign-to-port)
    - [assign IP to VLAN](#assign-ip-to-vlan)
- [Appendix](#appendix-1)
- [to take full backup](#to-take-full-backup)
  - [Configure HP Switch (5130)](#configure-hp-switch-5130)
    - [Ansible Modules](#ansible-modules)
    - [Configuring password authentication for console login](#configuring-password-authentication-for-console-login)
    - [Configuring SSH login on the device](#configuring-ssh-login-on-the-device)
    - [Install HP Comware 7 Python Library](#install-hp-comware-7-python-library)
    - [Install HP Ansible Modules](#install-hp-ansible-modules)
    - [HOW TO ENABLE NETCONF ON COMWARE7:](#how-to-enable-netconf-on-comware7)
    - [Check Comware Ansible Documentation](#check-comware-ansible-documentation)
    - [Assign port with VLAN](#assign-port-with-vlan)
- [Appendix](#appendix-2)
- [Troubleshooting](#troubleshooting)
  - [SSH Error](#ssh-error)
  - [discovered_interpreter_python": "/usr/bin/python"](#discovered_interpreter_python-usrbinpython)
  - [Ansible with netconf](#ansible-with-netconf)
  - [Enable scp on IOS device](#enable-scp-on-ios-device)
  - [References](#references)

<!-- /TOC -->


# Ansible for Network Automation

- install GNS3 GUI
- install and setup GNS3 VM

## First Lab to test GNS3 setup
- Create a lab with 2 PC and 1 SW
- add IP address for PCs
```
PC1> ip 10.1.1.1 255.255.255.0 
```
- test a ping from one PC to another
- remember to `save` device configuration

## Importing image to GNS3
Goto Edit -> Preferences -> Dynamips -> ISO Routers
  - click New -> Browse the image (bin) 
  - Choose Yes when ask for decompress
  - remember to refer the Cisco/GNS3 site for minimum memory requirement

## Importing appliance to GNS3
https://docs.gns3.com/1_3RdgLWgfk4ylRr99htYZrGMoFlJcmKAAaUAc8x9Ph8/index.html
- Download applicance file from website

## Configure a router

```
sh ip int br                    # show ip interface details
conf t                          # configure terminal
copy running-config startup-config
                                # save running configuration to startup
```


## NetworkAutomation Component
- download **NetworkAutomation** Component appliance from marketpalce which is a docker image with Ansible pre-installed.

## Ansible Network Modules and Adhoc commands
```
root@NetworkAutomation-1:~# ansible S1 -m raw -a "show version" -u david -k
```

Ref:
- https://docs.ansible.com/ansible/latest/modules.html
- https://docs.ansible.com/ansible/latest/modules_by_category.html
- https://docs.ansible.com/ansible/latest/raw_module.html

Ansible Ad Hoc commands:
========================
```
root@NetworkAutomation-1:~# ansible S1 -m raw -a "show version" -u david -k

root@NetworkAutomation-1:~# ansible S1 -m raw -a "show run" -u david -k

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show version" -u david -k

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show version" -u david -k | grep flash0

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show version" -u david -k | grep 'SUCCESS\|Software'

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show version" -u david -k | grep 'SUCCESS\|Version'

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show run" -u david -k | grep 'username'

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show run" -u david -k | grep 'SUCCESS\|username'
```

```
root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show run" -u david -k | grep 'username' > usernames.txt

root@NetworkAutomation-1:~# cat usernames.txt

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show run" -u david -k > shrun.txt

root@NetworkAutomation-1:~# cat shrun.txt

root@NetworkAutomation-1:~# more shrun.txt

root@NetworkAutomation-1:~# ansible gns3-core -i ./gns3hosts -m raw -a "show ver" -u david -k > shver.txt

root@NetworkAutomation-1:~# more shver.txt | grep Version

root@NetworkAutomation-1:~# more shver.txt | grep 'SUCCESS\|Version'

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show arp" -u david -k 

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show arp" -u david -k | grep 71

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show arp" -u david -k | grep 'SUCCESS\|71'

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show arp" -u david -k | grep 'SUCCESS\|\.71'

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show mac address-table" -u david -k 

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show mac address-table" -u david -k | grep 7a

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show mac address-table" -u david -k | grep 'SUCCESS\|fe7a'

root@NetworkAutomation-1:~# ansible all -i gns3hosts -m raw -a "show mac address-table" -u david -k | grep 'SUCCESS\|fe7a'

root@NetworkAutomation-1:~# cat gns3hosts
```


# Ansible CLI Playbook


# Appendix

HP : https://docs.gns3.com/appliances/hp-vsr1001.html

Nested Virtualization - VirtualBox
https://www.virtualbox.org/manual/ch09.html#nested-virt


## Driver is probably stuck stopping/starting

Steps:

Navigate to "C:\Program Files\Oracle\VirtualBox\drivers\vboxdrv"
Right click on "VBoxDrv.inf" file and select Install option
Open the Console as a administrator and run the following command


## vboxmanage cli
https://blog.scottlowe.org/2016/11/10/intro-to-vbox-cli/

vboxmanage startvm k8s
vboxmanage list runningvms

https://www.virtualbox.org/manual/ch08.html#vboxmanage-modifyvm

vboxmanage modifyvm Ubuntu --nested-hw-virt on

## Instal vmware workstation ubuntu
https://phoenixnap.com/kb/install-vmware-workstation-ubuntu


## Add IP to VM
sudo ip addr add 192.168.1.14/24 dev eth0
sudo ip link set dev eth0 up
sudo ip route add default via 192.168.1.1

sudo vi /etc/network/interfaces

sudo /etc/init.d/networking restart

### To configure a dynamic or Static IP address

```
auto eth0
iface eth0 inet dhcp

## Or configure a static IP
auto eth0
iface eth0 inet static
  address 192.168.1.14
  gateway 192.168.1.1
  netmask 255.255.255.0
  network 192.168.1.0
  broadcast 192.168.1.255

```

### Configure VLAN with IP

```
conf t
int vlan 100
ip address IP subnet
```

### VLANs
```
show vlans

ac-02#vlan database 
ac-02(vlan)# 
 

ac-02(config)#int vlan 100            
                            # create vlan interface
ac-02(config)#no interface vlan 100
                            # delete vlan interface

```

### assign to port
```
ac-02(config)#interface range fastEthernet 1/0 - 5
ac-02(config-if-range)#switchport mode access 
ac-02(config-if-range)#switchport access vlan 100

```

### assign IP to VLAN
```
ac-02(config)#interface vlan 100
ac-02(config-if)#ip address 10.1.10.70 255.255.255.0
```

# Appendix
https://srijit.com/working-cisco-ios-gns3/


# to take full backup
terminal length 0
show run



## Configure HP Switch (5130)
https://support.hpe.com/hpsc/doc/public/display?docId=emr_na-c03182828#N10013
http://patg.net/ansible,comware,switches/2014/10/16/ansible-comware/
https://porter.io/github.com/HPENetworking/ansible-hpe-cw7

### Ansible Modules
- https://hp-ansible.readthedocs.io/en/latest/list_of_All_modules.html
- https://github.com/HPENetworking/ansible-hpe-cw7
  
### Configuring password authentication for console login
```
<HPE> system-view
[HPE] line aux 0
[HPE-line-aux0] authentication-mode password

# Password is password
[HPE-line-aux0] set authentication password simple <PASSWORD>
[HPE-line-aux0] user-role network-admin

# save config
[HPE-line-aux0] save force
```

SSH on vty
```
[ds-01]line vty 0
[ds-01-line-vty0]authentication-mode scheme 
[ds-01-line-vty0]set authentication password simple password
[ds-01-line-vty0]protocol inbound ssh
[ds-01-line-vty0]user-role network-admin
```

### Configuring SSH login on the device
1. Create a key
```
<ds-01>system-view 
System View: return to User View with Ctrl+Z.
[ds-01]public-key local create rsa name ansiblekey
The range of public key modulus is (512 ~ 2048). 
If the key modulus is greater than 512, it will take a few minutes.
Press CTRL+C to abort.
Input the modulus length [default = 1024]:
Generating Keys...
.
Create the key pair successfully.
```

2. Enable SSH
```
[ds-01]ssh server enable
[ds-01]ssh user hp service-type all authentication-type password
[ds-01]netconf ssh server enable
```
```
[ds-01]sftp server enable
scp server enable
```
Enabling NETCONF over SSH
[ds-01]sftp server enable

Configuring the user lines for SSH login

Configuring a client's host public key
[ds-01]display public-key local rsa public
then find the key and copy for next command

[ds-01]public-key peer ansible-nw
Enter public key view. Return to system view with "peer-public-key end" command.
[ds-01-pkey-public-key-ansible-nw] <enter key here>
[ds-01-pkey-public-key-ansible-nw]peer-public-key end


3. Create an SSH user 
```
#
local-user hp
 password simple hp123
 service-type ssh http https
 authorization-attribute user-role network-admin
#
line vty 0 15
 authentication-mode scheme
 user-role network-admin
```

And specify the authentication mode. By default, no SSH user is configured on the device.
```
[ds-01]ssh user ansible service-type stelnet authentication-type password
```

1. Enter VTY line view or class view.
   
```
[ds-01]line vty 0
[ds-01-line-vty0]authentication-mode scheme
[ds-01-line-vty0]protocol inbound ssh
```

### Install HP Comware 7 Python Library
While in a terminal session on your Linux machine, execute one of the following blocks of commands:

Latest From Source
```
$ git clone https://github.com/HPENetworking/pyhpecw7.git
$ cd pyhpecw7
$ sudo python setup.py install
```
Latest Stable Release via PIP (not supported yet)

$ sudo pip install pyhpecw7

### Install HP Ansible Modules
First go back to your home directory.

$ cd
Perform a clone of this project.

$ git clone https://github.com/HPENetworking/ansible-hpe-cw7
Navigate to the new hp-ansible directory.

$ cd hp-ansible
### HOW TO ENABLE NETCONF ON COMWARE7:

https://networkgeekstuff.com/networking/hp-networking-comware-netconf-interface-quick-tutorial-using-pythons-ncclient-and-pyhpecw7/

Simple, here is a configuration snapshot that actually enables both NETCONF over SSH layer and creates a single user “admin” with password “admin” to access it.

```
ssh server enable
netconf ssh server enable

local-user admin class manage
 password simple admin
 service-type telnet ssh terminal
 authorization-attribute user-role network-admin

line vty 0 15
 authentication-mode scheme
 user-role network-operator
 idle-timeout 15 0
```

### Check Comware Ansible Documentation
`$ ansible-doc -M library/ comware_vlan`


### Assign port with VLAN

```
[ds-01]interface ge3/0
[ds-01-GigabitEthernet3/0]port link-mode bridge
[ds-01-GigabitEthernet3/0]port link-type access 
[ds-01-GigabitEthernet3/0]port access vlan 100

```

# Appendix
- [Connect GNS3 to the Internet (local server)](https://docs.gns3.com/1vFs-KENh2uUFfb47Q2oeSersmEK4WahzWX-HrMIMd00/index.html)

# Troubleshooting
https://docs.ansible.com/ansible/latest/network/user_guide/network_debug_troubleshooting.html

## SSH Error

```
root@NW-Auto-100:~/ansible# ssh cisco@\10.1.10.51
ssh: error while loading shared libraries: libgssapi_krb5.so.2: cannot open shared object file: No such file or directory

## Solution

root@NW-Auto-100:~/ansible# /sbin/ldconfig -v
```

## discovered_interpreter_python": "/usr/bin/python"

```
fatal: [ac-02]: FAILED! => {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python"}, "changed": false, "msg": "[Errno -2] Name or service not known"}
```
- https://www.toptechskills.com/ansible-tutorials-courses/how-to-fix-usr-bin-python-not-found-error-tutorial/

## Ansible with netconf

- [Netconf enabled Platform Options](https://docs.ansible.com/ansible/latest/network/user_guide/platform_netconf_enabled.html)


## Enable scp on IOS device
scp ~/Downloads/c2960x-universalk9-mz.152-2.E5.bin sysadmin1@192.0.2.1:flash2:/
Password:
Administratively disabled.

To fix that, get into config mode and run:

ip scp server enable

Ref: https://www.ispcolohost.com/2016/08/16/scping-an-ios-image-to-a-cisco-2960xr-stack/


- sometimes it happens due to encryption or integrity mismatch between SCP server and ASA. so you can support all cipher methods by the following 2 command (I face the same issue and TAC fixed bu those command)
- 
```
ssh cipher encryption all

ssh cipher integrity all
```

## References
- [Getting Started with Ansible for Network Automation](https://docs.ansible.com/ansible/latest/network/getting_started/index.html)
- [Ansible for Network Automation](https://docs.ansible.com/ansible/latest/network/index.html)
- [Ansible Network Examples](https://docs.ansible.com/ansible/latest/network/user_guide/network_best_practices_2.5.html)
- [Advanced Topics with Ansible for Network Automation](https://docs.ansible.com/ansible/latest/network/user_guide/)
- [Platform Options](https://docs.ansible.com/ansible/latest/network/user_guide/platform_index.html)
- [Netconf enabled Platform Options](https://docs.ansible.com/ansible/latest/network/user_guide/platform_netconf_enabled.html)
- [Ansible for Network Automation Tutorial](https://www.networkcomputing.com/networking/ansible-network-automation-tutorial)