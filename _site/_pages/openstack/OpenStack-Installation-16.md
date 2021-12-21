
```shell
# login to bastion
$ ssh your-account@workstation-GUID.domain.com

# create ssh keys
[student@workstation ~]$ ssh-keygen
#copy to undercloud node
[student@workstation ~]$ ssh-copy-id root@undercloud.example.com

$ ssh root@undercloud.example.com
# check hostname, ip, network

#create user
[root@undercloud ~]# useradd stack
[root@undercloud ~]# mkdir /home/stack/.ssh
[root@undercloud ~]# cp /root/.ssh/authorized_keys /home/stack/.ssh/
[root@undercloud ~]# chown -R stack:stack /home/stack/.ssh
[root@undercloud ~]# echo 'stack ALL=(root) NOPASSWD:ALL' | tee -a /etc/sudoers.d/stack
[root@undercloud ~]# chmod 0440 /etc/sudoers.d/stack

# login as stack
[student@workstation ~]$ ssh stack@undercloud.example.com
[stack@undercloud ~]$ sudo -i

# set channels
[root@undercloud ~]# cat >/etc/yum.repos.d/open.repo <<\EOF
[rhel-8-for-x86_64-appstream-eus-rpms]
name=rhel-8-for-x86_64-appstream-eus-rpms
baseurl=http://192.0.2.253/repos/rhel-8-for-x86_64-appstream-eus-rpms
enabled=1
gpgcheck=0

[rhel-8-for-x86_64-baseos-eus-rpms]
name=rhel-8-for-x86_64-baseos-eus-rpms
baseurl=http://192.0.2.253/repos/rhel-8-for-x86_64-baseos-eus-rpms
enabled=1
gpgcheck=0

[rhel-8-for-x86_64-highavailability-eus-rpms]
name=rhel-8-for-x86_64-highavailability-eus-rpms
baseurl=http://192.0.2.253/repos/rhel-8-for-x86_64-highavailability-eus-rpms
enabled=1
gpgcheck=0

[openstack-16.1-for-rhel-8-x86_64-rpms]
name=openstack-16.1-for-rhel-8-x86_64-rpms
baseurl=http://192.0.2.253/repos/openstack-16.1-for-rhel-8-x86_64-rpms
enabled=1
gpgcheck=0

[fast-datapath-for-rhel-8-x86_64-rpms]
name=fast-datapath-for-rhel-8-x86_64-rpms
baseurl=http://192.0.2.253/repos/fast-datapath-for-rhel-8-x86_64-rpms
enabled=1
gpgcheck=0


[ansible-2.9-for-rhel-8-x86_64-rpms]
name=ansible-2.9-for-rhel-8-x86_64-rpms
baseurl=http://192.0.2.253/repos/ansible-2.9-for-rhel-8-x86_64-rpms
enabled=1
gpgcheck=0

[rhceph-4-tools-for-rhel-8-x86_64-rpms]
name=rhceph-4-tools-for-rhel-8-x86_64-rpms
baseurl=http://192.0.2.253/repos/rhceph-4-tools-for-rhel-8-x86_64-rpms
enabled=1
gpgcheck=0

EOF

[root@undercloud ~]# yum makecache
[root@undercloud ~]# yum repolist
[root@undercloud ~]# yum -y update
[root@undercloud ~]# reboot
```

## Install Software

```shell
[root@undercloud ~]# yum -y install python3-tripleoclient
[root@undercloud ~]# yum -y install ceph-ansible
```

## Configure Undercloud Installation

```shell
# sample : undercloud.conf file in the stack user’s home directory. Red Hat provides a sample template (/usr/share/python-tripleoclient/undercloud.conf.sample)

# create /home/stack/undercloud.conf
[DEFAULT]
undercloud_hostname = undercloud.example.com
container_images_file = containers-prepare-parameter.yaml
local_ip = 192.0.2.1/24
undercloud_public_host = 192.0.2.2
undercloud_admin_host = 192.0.2.3
undercloud_nameservers = 192.0.2.254
#undercloud_ntp_servers =
#overcloud_domain_name = example.com
subnets = ctlplane-subnet
local_subnet = ctlplane-subnet
#undercloud_service_certificate =
generate_service_certificate = true
certificate_generation_ca = local
local_interface = eth0
inspection_extras = false
undercloud_debug = false
enable_tempest = false
enable_ui = false
hieradata_override = /home/stack/hieradata.yaml

[auth]

[ctlplane-subnet]
cidr = 192.0.2.0/24
dhcp_start = 192.0.2.5
dhcp_end = 192.0.2.24
inspection_iprange = 192.0.2.100,192.0.2.120
gateway = 192.0.2.254
```

```shell
## create /home/stack/hieradata.yaml
ironic::config::ironic_config:
  ipmi/use_ipmitool_retries:
    value: True
```

## Install Undercloud and Verify Installation

```shell
# Generate the containers-prepare-parameter.yaml
[stack@undercloud ~]$ openstack tripleo container image \
  prepare default  \
  --local-push-destination   \
  --output-env-file containers-prepare-parameter.yaml
# adjust the registry details and items as needed
```

```shell
# Install the undercloud:
[stack@undercloud ~]$ time openstack undercloud install
```

Check password and details
```shell
[stack@undercloud ~]$ cat ~/stackrc
[stack@undercloud ~]$ cat ~/undercloud-passwords.conf

[stack@undercloud ~]$ source ~/stackrc
(undercloud) [stack@undercloud ~]$ openstack catalog list
```

## Network Configuration Changes

```shell
(undercloud) [stack@undercloud ~]$ ip a
(undercloud) [stack@undercloud ~]$ ip r
(undercloud) [stack@undercloud ~]$ sudo ovs-vsctl show
(undercloud) [stack@undercloud ~]$ cat /etc/os-net-config/config.json | python3 -m json.tool
```

### Undercloud Neutron Subnet

```shell
(undercloud) [stack@undercloud ~]$ openstack network list
(undercloud) [stack@undercloud ~]$ openstack subnet list
(undercloud) [stack@undercloud ~]$ openstack subnet show ctlplane-subnet
```

## OverCloud Images

```shell
(undercloud) [stack@undercloud ~]$ mkdir images
(undercloud) [stack@undercloud ~]$ mkdir -p templates/environments
(undercloud) [stack@undercloud ~]$ sudo yum -y install rhosp-director-images

(undercloud) [stack@undercloud ~]$ tar -C images -xvf /usr/share/rhosp-director-images/overcloud-full-latest.tar(undercloud) [stack@undercloud ~]$ tar -C images -xvf /usr/share/rhosp-director-images/ironic-python-agent-latest.tar

## upload image
(undercloud) [stack@undercloud ~]$ openstack overcloud image upload --image-path ~/images
## check images
(undercloud) [stack@undercloud ~]$ openstack image list
## check content of httpboot
(undercloud) [stack@undercloud ~]$ ls -al /var/lib/ironic/httpboot/
(undercloud) [stack@undercloud ~]$ cat /var/lib/ironic/httpboot/inspector.ipxe
```

### Deploy Containerized Overcloud

```shell
## list repo
(undercloud) [stack@undercloud ~]$ curl -s -H "Accept: application/json" http://192.0.2.1:8787/v2/_catalog | python3 -m json.tool
```

## Overcloud Deployment Preparation

```shell
## create nodes.json
{
    "nodes": [
        {
            "mac": [
                "2c:c2:60:01:02:02"
            ],
            "name": "ctrl01",
            "pm_addr": "192.0.2.221",
            "pm_password": "redhat",
            "pm_type": "pxe_ipmitool",
            "pm_user": "admin"
        },
        {
            "mac": [
                "2c:c2:60:01:02:03"
            ],
            "name": "ctrl02",
            "pm_addr": "192.0.2.222",
            "pm_password": "redhat",
            "pm_type": "pxe_ipmitool",
            "pm_user": "admin"
        },
        {
            "mac": [
                "2c:c2:60:01:02:04"
            ],
            "name": "ctrl03",
            "pm_addr": "192.0.2.223",
            "pm_password": "redhat",
            "pm_type": "pxe_ipmitool",
            "pm_user": "admin"
        },
        {
            "mac": [
                "2c:c2:60:01:02:05"
            ],
            "name": "compute01",
            "pm_addr": "192.0.2.224",
            "pm_password": "redhat",
            "pm_type": "pxe_ipmitool",
            "pm_user": "admin"
        },
        {
            "mac": [
                "2c:c2:60:01:02:06"
            ],
            "name": "compute02",
            "pm_addr": "192.0.2.225",
            "pm_password": "redhat",
            "pm_type": "pxe_ipmitool",
            "pm_user": "admin"
        }
    ]
}

## check if any baremetal
(undercloud) [stack@undercloud ~]$ openstack baremetal node list

## validate
(undercloud) [stack@undercloud ~]$ openstack overcloud node import --validate-only ~/nodes.json
## import nodes.json and perform interospection
(undercloud) [stack@undercloud ~]$ openstack overcloud node import --introspect --provide nodes.json

## check berametals again
(undercloud) [stack@undercloud ~]$ openstack baremetal node list
(undercloud) [stack@undercloud ~]$ openstack baremetal node show ctrl01
(undercloud) [stack@undercloud ~]$ openstack baremetal node show ctrl01 -f json -c driver_info
(undercloud) [stack@undercloud ~]$ openstack baremetal introspection list
(undercloud) [stack@undercloud ~]$ openstack baremetal node show ctrl01 -f json -c properties

## review all info
(undercloud) [stack@undercloud ~]$ openstack baremetal introspection data save ctrl01 | jq "."
```

## Overcloud Templates Preparation

```shell
(undercloud) [stack@undercloud ~]$ mkdir -p ~/templates/environments

## create custom environment file - /home/stack/templates/environments/node-info.yaml
parameter_defaults:
  OvercloudControlFlavor: baremetal
  OvercloudComputeFlavor: baremetal
  ControllerCount: 3
  ComputeCount: 2

## prepare env file -/home/stack/templates/environments/fix-nova-reserved-host-memory.yaml
parameter_defaults:
  NovaReservedHostMemory: 1024
```

### Create Custom Network Environment File

```shell
(undercloud) [stack@undercloud ~]$ THT=/usr/share/openstack-tripleo-heat-templates
(undercloud) [stack@undercloud ~]$ cp $THT/roles_data.yaml ~/templates
(undercloud) [stack@undercloud ~]$ cp $THT/network_data.yaml ~/templates

## replace content of ~/templates/network_data.yaml
- name: Storage
  vip: true
  vlan: 30
  name_lower: storage
  ip_subnet: '172.18.0.0/24'
  allocation_pools: [{'start': '172.18.0.11', 'end': '172.18.0.250'}]
  ipv6_subnet: 'fd00:fd00:fd00:3000::/64'
  ipv6_allocation_pools: [{'start': 'fd00:fd00:fd00:3000::10', 'end': 'fd00:fd00:fd00:3000:ffff:ffff:ffff:fffe'}]
- name: StorageMgmt
  name_lower: storage_mgmt
  vip: true
  vlan: 40
  ip_subnet: '172.19.0.0/24'
  allocation_pools: [{'start': '172.19.0.11', 'end': '172.19.0.250'}]
  ipv6_subnet: 'fd00:fd00:fd00:4000::/64'
  ipv6_allocation_pools: [{'start': 'fd00:fd00:fd00:4000::10', 'end': 'fd00:fd00:fd00:4000:ffff:ffff:ffff:fffe'}]
- name: InternalApi
  name_lower: internal_api
  vip: true
  vlan: 20
  ip_subnet: '172.17.0.0/24'
  allocation_pools: [{'start': '172.17.0.11', 'end': '172.17.0.250'}]
  ipv6_subnet: 'fd00:fd00:fd00:2000::/64'
  ipv6_allocation_pools: [{'start': 'fd00:fd00:fd00:2000::10', 'end': 'fd00:fd00:fd00:2000:ffff:ffff:ffff:fffe'}]
- name: Tenant
  vip: false  # Tenant network does not use VIPs
  name_lower: tenant
  vlan: 50
  ip_subnet: '172.16.0.0/24'
  allocation_pools: [{'start': '172.16.0.11', 'end': '172.16.0.250'}]
  # Note that tenant tunneling is only compatible with IPv4 addressing at this time.
  ipv6_subnet: 'fd00:fd00:fd00:5000::/64'
  ipv6_allocation_pools: [{'start': 'fd00:fd00:fd00:5000::10', 'end': 'fd00:fd00:fd00:5000:ffff:ffff:ffff:fffe'}]
- name: External
  vip: true
  name_lower: external
  vlan: 10
  ip_subnet: '10.0.0.0/24'
  allocation_pools: [{'start': '10.0.0.201', 'end': '10.0.0.250'}]
  gateway_ip: '10.0.0.251'
  ipv6_subnet: '2001:db8:fd00:1000::/64'
  ipv6_allocation_pools: [{'start': '2001:db8:fd00:1000::10', 'end': '2001:db8:fd00:1000:ffff:ffff:ffff:fffe'}]
  gateway_ipv6: '2001:db8:fd00:1000::1'
- name: Management
  # Management network is enabled by default for backwards-compatibility, but
  # is not included in any roles by default. Add to role definitions to use.
  enabled: true
  vip: false  # Management network does not use VIPs
  name_lower: management
  vlan: 60
  ip_subnet: '10.0.1.0/24'
  allocation_pools: [{'start': '10.0.1.4', 'end': '10.0.1.250'}]
  ipv6_subnet: 'fd00:fd00:fd00:6000::/64'
  ipv6_allocation_pools: [{'start': 'fd00:fd00:fd00:6000::10', 'end': 'fd00:fd00:fd00:6000:ffff:ffff:ffff:fffe'}]

## generate env
(undercloud) [stack@undercloud ~]$ mkdir ~/workplace
(undercloud) [stack@undercloud ~]$ mkdir ~/output
(undercloud) [stack@undercloud ~]$ cp -rp /usr/share/openstack-tripleo-heat-templates/* workplace
(undercloud) [stack@undercloud ~]$ cd workplace

## process TripleO Heat Templates
(undercloud) [stack@undercloud workplace]$ tools/process-templates.py -r ../templates/roles_data.yaml -n ../templates/network_data.yaml -o ../output

(undercloud) [stack@undercloud workplace]$ cd ../output
(undercloud) [stack@undercloud output]$ cat environments/network-environment.yaml

## copy network env file 
(undercloud) [stack@undercloud output]$ cp environments/network-environment.yaml ~/templates/environments

(undercloud) [stack@undercloud output]$ cd ~
(undercloud) [stack@undercloud ~]$ sed -i 's/single-nic-vlans/multiple-nics/' templates/environments/network-environment.yaml
(undercloud) [stack@undercloud ~]$ grep -A1 SoftwareConfig templates/environments/network-environment.yaml
```

### Create Network Interface Configuration Templates

```shell
(undercloud) [stack@undercloud ~]$ mkdir -p ~/templates/network/config/multiple-nics/
(undercloud) [stack@undercloud ~]$ cp ~/output/network/config/multiple-nics/*.yaml ~/templates/network/config/multiple-nics/
```
