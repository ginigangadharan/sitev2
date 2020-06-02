# Setup your OpenStack Infra

## Install OS
Install CentOS7/RHEL7 with minimal installation type

## Update
yum update -y

## Update /etc/hosts
Ensure the hostname entry in `/etc/hosts` file
hostnamectl set-hostname node1.lab.local

## Stop and Disable Firewall
```
systemctl disable firewalld;systemctl stop firewalld;systemctl status firewalld
```

## Stop and Disable NetworkManager
(Don't do this step in your laptop as you may lose your Wired/Wireless connection. This step is only applicable on Production servers)
```
systemctl disable NetworkManager;systemctl stop NetworkManager;systemctl status NetworkManager
```

## Verify Network Connectivity
Now check the internet connectivity from the linux machine by the way of pinging some website from linux machine.

## Update SELinux and Verify
Change the SElinux, edit and update value to `SELINUX=permissive
To check the current selinux status (if you still see enforcing then reboot your machine)
```
# setenforce 0
# getenforce
Permissive
```

## Install epel Repo
Install epel-release repository ; This repository will help to install all packages
(If you are using RHEL Subscribed version, no need to do this.)

```
yum install epel-release
# or
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

## Install Necessary Packages
(Skip this steps if you are using RHEL Subscribed version)
```
yum install erlang* vim -y
```

## NTP Installation
```
yum install ntp -y
systemctl start ntpd
systemctl enable ntpd
systemctl status ntpd
```

**Force linux machine to sync with NTP server**
```
ntpdate -q 0.ro.pool.ntp.org 1.ro.pool.ntp.org
```

## Install openstack repository 
(Skip this steps if you are using RHEL Subscribed version)

```
yum install https://repos.fedorapeople.org/openstack/openstack-liberty/rdo-release-liberty-5.noarch.rpm
# or 
http://mirror.centos.org/centos/7/cloud/x86_64/openstack-mitaka/
```
*https://repos.fedorapeople.org/openstack/openstack-mitaka/rdo-release-mitaka-7.noarch.rpm*

## Remove epel Repo
(Skip this steps if you are using RHEL Subscribed version)
Since erlang package is getting conflict with openstack,remove the epel repo as per the following steps.
```
# cd /etc/yum.repos.d/
# mv epel* /root
# yum clean all
# yum repolist
```

## Install openvswitch
```
yum install -y openvswitch
systemctl start openvswitch
systemctl enable openvswitch
systemctl status openvswitch
```

Check the current bridge or interface available in openvswitch
```
[root@opstack yum.repos.d]# ovs-vsctl show
88d29945-a8b9-4b4d-92ac-1b4316d29178
    ovs_version: "2.5.0"
```
Create bridge in the linux before start the openstack
```
# ovs-vsctl add-br br-ex

# ovs-vsctl show
88d29945-a8b9-4b4d-92ac-1b4316d29178
    Bridge br-ex
        Port br-ex
            Interface br-ex
                type: internal
    ovs_version: "2.5.0"
```

## Configure NIC card configuration files
```
# vim /etc/sysconfig/network-scripts/ifcfg-eno16777736
```
where eno16777736 is your physical network interface

Make content as below
```
TYPE=OVSPort
ONBOOT=yes
DEVICETYPE=ovs
OVS_BRIDGE=br-ex
DEVICE=eno16777736
NAME=eno16777736
BOOTPROTO=none
```
Configure bridge
```
# vim /etc/sysconfig/network-scripts/ifcfg-br-ex
```
Make content as below
```
DEVICE=br-ex
DEVICETYPE=ovs
TYPE=OVSIntPort
BOOTPROTO=static
OVS_BRIDGE=br-ex
IPADDR=192.168.0.11
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
DNS1=192.168.0.1
ONBOOT=yes
```
Associate the bridge to the physical network card Make sure you execute both commands together otherwise you may lose the connectivity.
```
# ovs-vsctl add-port br-ex eno16777736;systemctl restart network
```
Check the NIC status
```
# ip a
```

Check vSwitch status
```
# ovs-vsctl show
[root@os network-scripts]# ovs-vsctl show
eccb252f-f4f1-43cc-8b84-42255c4e3912
	Bridge br-ex
    	Port "enp0s3"
        	Interface "enp0s3"
    	Port br-ex
        	Interface br-ex
            	type: internal
	ovs_version: "2.1.3"
```

## Install Dependency Packages
before start the actual openstack installation please install the package dependencies.....
```
yum install -y puppet hiera openssh-clients tar nc rubygem-json
```

## Install packstack
Check the openstack package now is available or not
```
[root@opstack yum.repos.d]# yum list *packstack*
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.excellmedia.net
 * extras: centos.excellmedia.net
 * updates: centos.excellmedia.net
Available Packages
openstack-packstack.noarch                                      8.0.2-1.el7                               openstack-mitaka
openstack-packstack-doc.noarch                                  8.0.2-1.el7                               openstack-mitaka
openstack-packstack-puppet.noarch                               8.0.2-1.el7                               openstack-mitaka
Install the packstack package
# yum install openstack-packstack
```

## Install Openstack
Create answer file to customise the openstack installation
```
packstack --gen-answer-file=/root/answers.txt
```
Modify answer file and update below parameters
```
CONFIG_DEFAULT_PASSWORD=redhat
CONFIG_SWIFT_INSTALL=y
CONFIG_HEAT_INSTALL=y
CONFIG_NTP_SERVERS=172.25.254.254
CONFIG_KEYSTONE_ADMIN_PW=redhat
CONFIG_CINDER_VOLUMES_CREATE=n
CONFIG_LBAAS_INSTALL=y
CONFIG_NEUTRON_METERING_AGENT_INSTALL=y
CONFIG_NEUTRON_FWAAS=y
CONFIG_NEUTRON_ML2_TYPE_DRIVERS=vlan
CONFIG_NEUTRON_ML2_TENANT_NETWORK_TYPES=vlan
CONFIG_NEUTRON_ML2_VLAN_RANGES=physnet1:1:1000
CONFIG_NEUTRON_OVS_BRIDGE_MAPPINGS=physnet1:br-eth1
CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-eth1:eth1
CONFIG_HORIZON_SSL=y
CONFIG_HEAT_CFN_INSTALL=y
CONFIG_PROVISION_DEMO=n
CONFIG_CINDER_INSTALL=y
```

Use the answer file and execute openstack install
```
packstack --answer-file /root/answers.txt
```
Output from Laptop :
```
Additional information:
 * File /root/keystonerc_admin has been created on OpenStack client host 192.168.0.165. To use the command line tools you need to source the file.
 * NOTE : A certificate was generated to be used for ssl, You should change the ssl certificate configured in /etc/httpd/conf.d/ssl.conf on 192.168.0.165 to use a CA signed cert.
 * To access the OpenStack Dashboard browse to https://192.168.0.165/dashboard .
Please, find your login credentials stored in the keystonerc_admin in your home directory.
 * To use Nagios, browse to http://192.168.0.165/nagios username: nagiosadmin, password: ea35bcc65dea43cd
 * The installation log file is available at: /var/tmp/packstack/20170609-002635-SZiNrX/openstack-setup.log
 * The generated manifests are available at: /var/tmp/packstack/20170609-002635-SZiNrX/manifests
```
 
# Openstack all services list
```
systemctl restart httpd.service                            	 
systemctl restart iscsi.service                            	 
systemctl restart ksm.service                              	 
systemctl restart ksmtuned.service                         	 

systemctl restart libvirtd.service
systemctl restart lvm2-monitor.service
systemctl restart mariadb.service
systemctl restart mdmonitor.service
systemctl restart memcached.service
systemctl restart mongod.service
systemctl restart netcf-transaction.service
systemctl restart neutron-dhcp-agent.service
systemctl restart neutron-l3-agent.service
systemctl restart neutron-metadata-agent.service
systemctl restart neutron-openvswitch-agent.service
systemctl restart neutron-ovs-cleanup.service
systemctl restart neutron-server.service
systemctl restart ntpd.service
systemctl restart openstack-ceilometer-alarm-evaluator.service
systemctl restart openstack-ceilometer-alarm-notifier.service
systemctl restart openstack-ceilometer-api.service
systemctl restart openstack-ceilometer-central.service

systemctl restart openstack-ceilometer-collector.service
systemctl restart openstack-ceilometer-compute.service
systemctl restart openstack-ceilometer-notification.service
systemctl restart openstack-cinder-api.service
systemctl restart openstack-cinder-backup.service
systemctl restart openstack-cinder-scheduler.service
systemctl restart openstack-cinder-volume.service
systemctl restart openstack-glance-api.service
systemctl restart openstack-glance-registry.service
systemctl restart openstack-keystone.service
systemctl restart openstack-losetup.service
systemctl restart openstack-nova-api.service
systemctl restart openstack-nova-cert.service
systemctl restart openstack-nova-compute.service
systemctl restart openstack-nova-conductor.service
systemctl restart openstack-nova-consoleauth.service
systemctl restart openstack-nova-novncproxy.service
systemctl restart openstack-nova-scheduler.service

systemctl restart openstack-swift-account-auditor.service
systemctl restart openstack-swift-account-reaper.service
systemctl restart openstack-swift-account-replicator.service
systemctl restart openstack-swift-account.service
systemctl restart openstack-swift-container-auditor.service
systemctl restart openstack-swift-container-replicator.service
systemctl restart openstack-swift-container-updater.service
systemctl restart openstack-swift-container.service
systemctl restart openstack-swift-object-auditor.service
systemctl restart openstack-swift-object-replicator.service
systemctl restart openstack-swift-object-updater.service
systemctl restart openstack-swift-object.service
systemctl restart openstack-swift-proxy.service
systemctl restart openvswitch.service
systemctl restart rabbitmq-server.service
```
