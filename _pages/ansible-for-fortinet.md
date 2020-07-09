---
layout: post
title: Ansible for FortiNet
author: gini
categories: [ ansible ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: [network lab, network automation, ansible network automation]
permalink: ansible-for-fortinet
featured: false
hidden: false
---

<!-- TOC -->autoauto- [image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"](#image-assetsimagesgini-redhat-cloudevent-2019-2jpg)auto- [show-avatar: false](#show-avatar-false)auto- [Setup FortiNet device on GNS3](#setup-fortinet-device-on-gns3)auto- [API References](#api-references)auto- [Web Rating Overrides](#web-rating-overrides)auto- [1.1. Prerequisites](#11-prerequisites)auto  - [1.1.1. Install fortiosAPI](#111-install-fortiosapi)auto  - [Install FortiOS Collection from Ansible Galaxy](#install-fortios-collection-from-ansible-galaxy)auto- [1.2. Configuring a URL filter](#12-configuring-a-url-filter)auto  - [1.2.1. To add a URL to a URL filter](#121-to-add-a-url-to-a-url-filter)auto- [Appendix](#appendix)auto- [Troubleshooting](#troubleshooting)auto  - [HTTPS issue](#https-issue)auto- [References](#references)autoauto<!-- /TOC -->


# Setup FortiNet device on GNS3

Refer : **[Fortinet Device Setup in GNS3](gns3-device-setup)**

# API References

- https://github.com/eoprede/ansible_fortios_api
- [Use REST API Access FortiGate](https://kb.fortinet.com/kb/documentLink.do?externalID=FD46329)

```
$ curl -k -i -X POST http://10.1.10.70/logincheck -d "username=admin&secretkey=password" --dump-header headers.txt -c cookies.txt
$ curl -k -i -X GET http://10.1.10.70/api/v2/cmdb/router/static -b headers.txt

### logout
$ curl -k -i -X POST http://10.1.10.70/logout
```

# 1.1. Prerequisites

## 1.1.1. Install fortiosAPI

```
pip install fortiosapi
#or
pip3 install fortiosapi
```
Ref: https://pypi.org/project/fortiosapi/

## Install FortiOS Collection from Ansible Galaxy

`ansible-galaxy collection install fortinet.fortios`

https://github.com/fortinet-ansible-dev/ansible-galaxy-fortios-collection/tree/fos_v6.0.0/galaxy_1.0.13


# Web Rating Overrides

Ref: [Overriding FortiGuard website categorization](https://help.fortinet.com/fos60hlp/60/Content/FortiOS/fortigate-security-profiles/Web_Filter/Overriding%20FortiGuard%20website%20categorization.htm)

## CLI commands

In the CLI, the term is local category.

To create a local category:
```
config webfilter ftgd-local-cat
  edit local_category_1
    set id 140
  end
```
eg:
```
config webfilter ftgd-local-cat
  edit custom_category
    set id 150
  end
```

To set a rating to a Local Category:
```
config webfilter ftgd-local-rating
  edit <url_str>
    set rating {[<category_int>] [group_str] . . .]
    set status {enable | disable}
  end
```
eg:
```
config webfilter ftgd-local-rating
  edit testurl101.com
    set rating 150
    set status enable
  end
```

## Using Ansible Modules

- [fortios_webfilter_ftgd_local_cat](https://docs.ansible.com/ansible/latest/modules/fortios_webfilter_ftgd_local_cat_module.html) - Configure FortiGuard Web Filter local categories in Fortinet’s FortiOS and FortiGate
- [fortios_webfilter_ftgd_local_rating](https://docs.ansible.com/ansible/latest/modules/fortios_webfilter_ftgd_local_rating_module.html) - Configure local FortiGuard Web Filter local ratings in Fortinet’s FortiOS and FortiGate


# Add the new member to blacklist group

https://help.fortinet.com/fos60hlp/60/Content/FortiOS/fortigate-firewall/Object%20Configuration/Addresses/Address%20Groups.htm?Highlight=add%20address%20to%20group

https://docs.fortinet.com/vm/cisco-aci/fortigate/5.6/sdn-connector/5.6.3/617358/configuring-the-firewall-address-and-address-group

```
config firewall address

  edit "test-tag"

    set type dynamic

    set sdn aci

    set tenant "TENANT-NAME"

    set epg-name "AP-NAME|EPG-NAME"

    set sdn-tag "TAG-NAME"

  next

end

config firewall addrgrp

  edit "test-group"

    set member "test-tag" "Adobe Login"

  next

end
```

```
FortiGate-VM64-KVM # show firewall addrgrp blklist1
config firewall addrgrp
    edit "blklist1"
        set uuid b257da0e-bd59-51ea-2067-4bd93d716b5f
        set member "block-10.6.10.0/24"
        set comment "Added via Ansible"
        set color 6
    next
end
```

# Appendix
```
config firewall address
edit "eg  block ip1"
        set subnet 192.168.82.82 255.255.255.255
    next
    edit "eg block ip2"
        set subnet 192.168.81.81 255.255.255.255
    next
end
config firewall addrgrp
	edit "Group of block ip"
        set member "eg  block ip1" "eg block ip2"
next
end
config webfilter urlfilter
 edit 1
        set name "webfilter-url"
        config entries
	edit 1
                set url "*"
                set type wildcard
                set action block
            next
	end
	next
end
config firewall policy
edit 1
        set name "Weburl filter"
        set srcintf "lan1"
        set dstintf "wan1"
        set srcaddr "Group of block ip"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set utm-status enable
        set inspection-mode proxy
        set logtraffic all
        set webfilter-profile "webfilter-url"    
        set profile-protocol-options "protocol"
        set ssl-ssh-profile "protocols"
        set nat enable
    next
end
```
## ansible-fortios-generic module 

Ref: [ansible-fortios-generic](https://github.com/fortinet/ansible-fortios-generic)

# Troubleshooting

## HTTPS issue
[SSL: WRONG_VERSION_NUMBER] wrong version number (_ssl.c:1076)')))

A - Disable https and use http



# References

https://docs.fortinet.com/document/fortigate/6.2.3/cookbook/954635/getting-started
https://ftnt-ansible-docs.readthedocs.io/en/latest/fortios_modules/fortios_webfilter_urlfilter.html
https://docs.ansible.com/ansible/latest/modules/fortios_webfilter_urlfilter_module.html
https://docs.ansible.com/ansible/latest/modules/fortios_firewall_policy_module.html

