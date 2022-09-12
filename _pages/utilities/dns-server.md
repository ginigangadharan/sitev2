---
layout: post
title: Configure DNS Server for Lab
author: gini
categories: [ DNS ]
tags: [dns]
permalink: dns-server
featured: false
hidden: false
image: "assets/images/tools-1426927308491-6380b6a9936f.jpeg"
titleshort: dns server
---



- [DNS Server using bind](#dns-server-using-bind)
  - [Install bind](#install-bind)
  - [Configure /etc/named.conf](#configure-etcnamedconf)
  - [Remove existing wrong zone if any](#remove-existing-wrong-zone-if-any)
  - [Create Forward Zone - /etc/named.conf](#create-forward-zone---etcnamedconf)
  - [Create Reverse Zone - /etc/named.conf](#create-reverse-zone---etcnamedconf)
  - [Create fwd zon file - /var/named/fwd.ansible.lab.local.db](#create-fwd-zon-file---varnamedfwdansiblelablocaldb)
  - [Create reverse zone db - /var/named/103.168.192.db](#create-reverse-zone-db---varnamed103168192db)
  - [Start named service](#start-named-service)
  - [Open firewall ports](#open-firewall-ports)
  - [Add in network and resolv.conf](#add-in-network-and-resolvconf)
- [Ansible Roles](#ansible-roles)
- [DNS Server using dnsmasq](#dns-server-using-dnsmasq)
  - [Enable firewall ports](#enable-firewall-ports)
- [Enable DHCP Server Using dnsmasq](#enable-dhcp-server-using-dnsmasq)

# DNS Server using bind

## Install bind
```
yum -y install bind bind-utils
```

## Configure /etc/named.conf
```
listen-on port 53 { 127.0.0.1; };

allow-query     { localhost; 192.168.103.0/24; };
```

## Remove existing wrong zone if any
Comment out if you are facing issue with default zone part.
```
#zone "." IN {
#       type hint;
#       file "named.ca";
#};
```

## Create Forward Zone - /etc/named.conf
```
zone "ansiblelab.local" IN {
         type master;
         file "fwd.ansiblelab.local.db";
         allow-update { none; };
         allow-query {any; };
};
```

## Create Reverse Zone - /etc/named.conf
```
zone "103.168.192.in-addr.arpa" IN {
          type master;
          file "103.168.192.db";
          allow-update { none; };
          allow-query {any; };
};
```


## Create fwd zon file - /var/named/fwd.ansible.lab.local.db
```
@   IN  SOA     fjpocans01.ansiblelab.local. root.ansiblelab.local. (
                                                1001    ;Serial
                                                3H      ;Refresh
                                                15M     ;Retry
                                                1W      ;Expire
                                                1D      ;Minimum TTL
                                                )

;Name Server Information
@      IN  NS      fjpocans01.ansiblelab.local.

;IP address of Name Server
primary IN  A       192.168.103.10
fjpocans01 IN A     192.168.103.10
;Mail exchanger
ansiblelab.local. IN  MX 10   mail.ansiblelab.local.

;A - Record HostName To IP Address
www     IN  A       192.168.103.100
mail    IN  A       192.168.103.150

;CNAME record
ftp     IN CNAME        www.ansiblelab.local.
```

*Whenever you update the zone lookup file, you need to change/increment the serial like 1002 ;Serial.*

## Create reverse zone db - /var/named/103.168.192.db
```
@   IN  SOA     fjpocans01.ansiblelab.local. root.nsiblelab.local. (
                                                1001    ;Serial
                                                3H      ;Refresh
                                                15M     ;Retry
                                                1W      ;Expire
                                                1D      ;Minimum TTL
                                                )

;Name Server Information
@ IN  NS      fjpocans01.ansiblelab.local.

;Reverse lookup for Name Server
10        IN  PTR     fjpocans01.ansiblelab.local.

;PTR Record IP address to HostName
100      IN  PTR     www.ansiblelab.local.
150      IN  PTR     mail.ansiblelab.local.

```

*Whenever you update the zone lookup file, you need to change/increment the serial like 1002 ;Serial.*

## Start named service
```
systemctl restart named
systemctl enable named
```

## Open firewall ports
```
firewall-cmd --permanent --add-port=53/udp
firewall-cmd --reload
```

## Add in network and resolv.conf

*Tip: Install BIND utilities 'yum install -y bind-utils' package to get nslookup or dig command.*


# Ansible Roles
https://galaxy.ansible.com/bertvv/bind



# DNS Server using dnsmasq 
https://www.tecmint.com/setup-a-dns-dhcp-server-using-dnsmasq-on-centos-rhel/


```
yum install dnsmasq

systemctl start dnsmasq
systemctl enable dnsmasq
systemctl status dnsmasq

configure 
# cat /etc/dnsmasq.conf  |grep -v ^# |grep -v ^$
domain-needed
bogus-priv
server=10.6.2.11
server=8.8.8.8
address=/lab.local/127.0.0.1
address=/lab.local/10.6.1.202
interface=eth0
listen-address=::1,127.0.0.1,10.6.1.202
bind-interfaces
expand-hosts
domain=lab.local
conf-dir=/etc/dnsmasq.d,.rpmnew,.rpmsave,.rpmorig


# dnsmasq --test
dnsmasq: syntax check OK.


Defining DNS Hosts and Names
The Dnsmasq reads all the DNS hosts and names from the /etc/hosts file, so add your DNS hosts IP addresses and name pairs as shown.


# 5. cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.6.1.194  node1.lab.local node1
10.6.1.195  node2.lab.local node2
10.6.1.196  node3.lab.local node3
10.6.1.197  node4.lab.local node4
10.6.1.199  ovirt-engine-02.lab.local ovirt-engine-02
10.6.1.200  vm-ansible-01.lab.local vm-ansible-01
10.6.1.202  vm-utils-01.lab.local vm-utils-01 ns1.labl.local ns1 dns.lab.local dns repo.lab.local repo
10.6.1.196  node3.lab.local node3
10.6.1.198  vcenter.lab.local vcenter
```

## Enable firewall ports

```
firewall-cmd --add-service=dns --permanent
firewall-cmd --add-service=dhcp --permanent
firewall-cmd --reload
```

# Enable DHCP Server Using dnsmasq

```
# cat /etc/dnsmasq.conf |grep -v ^# |egrep "dhcp-range|dhcp-option|dhcp-leasefile|dhcp-authoritative"
dhcp-range=10.6.1.207,10.6.1.210,255.255.255.128,12h
dhcp-option=3,10.6.1.129
dhcp-leasefile=/var/lib/dnsmasq/dnsmasq.leases
dhcp-authoritative
```

`sudo systemctl restart dnsmasq`
