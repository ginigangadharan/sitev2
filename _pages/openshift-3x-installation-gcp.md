---
layout: post
title: OCP Origin 3.x Installation - on GCP
categories: [ openshift ]
tags: [ OpenShift Installation ]
show-avatar: false
permalink: /openshift-3x-installation-gcp
featured: false
hidden: false
---

<!-- TOC -->

- [Master Server Configuration](#master-server-configuration)
	- [Step 1 : Update system](#step-1--update-system)
	- [Step 2 : Install Required Packages](#step-2--install-required-packages)
	- [Step 3 : Clone the package from github](#step-3--clone-the-package-from-github)
	- [Step 4: Configure your DNS server](#step-4-configure-your-dns-server)
	- [Step 5 : Create a user with admin privilege](#step-5--create-a-user-with-admin-privilege)
	- [Step 6 : Start and Enable Docker](#step-6--start-and-enable-docker)
	- [Step 7 : Configure Storage](#step-7--configure-storage)
	- [Step 8 : Configure the inventory file](#step-8--configure-the-inventory-file)
	- [Step 9: Verify Pre-Req](#step-9-verify-pre-req)
	- [Step 10: Deploy Cluster](#step-10-deploy-cluster)
	- [Step 11: Configure cluster admin login](#step-11-configure-cluster-admin-login)
	- [Step 12: Verify Cluster Status](#step-12-verify-cluster-status)
	- [Step 13: Verify basic pod status](#step-13-verify-basic-pod-status)
	- [Step 14: Verify Web Console](#step-14-verify-web-console)
	- [Restart or Check Status of openshift service](#restart-or-check-status-of-openshift-service)
	- [Some more configs - Configuring Host Variables](#some-more-configs---configuring-host-variables)
	- [Verify PublicURL](#verify-publicurl)
	- [Uninstalling OCP Cluster](#uninstalling-ocp-cluster)
- [Appendix](#appendix)
	- [References](#references)

<!-- /TOC -->


This is a quick guide to setup OpenShift Origin Cluster with 1 Master and 2 Nodes. We will use ansible playbooks provided by https://github.com/OpenShift/OpenShift-ansible. You need to setup a separate DNS server to manage internal DNS traffic.

**Master server hosted on GCP**

Components running on the server

- kubernetes API Server
- etcd
- Controller Manager Server
- Ansible
- Docker Service
- OpenShift Container Registry
- Web console if any
- CLI Client

**Node 1 & 2: Hosted on GCP:**

- Docker service
- kubelet
- service proxy

**Important Notes:**

- In ansible inventory file, `ansible_service_broker_install=False`; if this entry is `true` then playbook run will fail always if your DNS server didn't configure properly.

- If DNS server is ready, use wild card entry as `*.master.sun.com`
  
	eg:
	`*.apps.master.sun.com. IN A 10.148.0.2`

- Configure static IP address on your Master servers and nodes.

# Master Server Configuration

Below see the steps to follow.

## Step 1 : Update system

```
yum update
reboot
```

## Step 2 : Install Required Packages

```
# yum install -y wget git zile nano net-tools \
	docker bind-utils iptables-services bridge-utils \
	bash-completion kexec-tools sos psacct openssl-devel \
	httpd-tools NetworkManager python-cryptography \
	python2-pip python-devel python-passlib \
	java-1.6.0-openjdk python-rhsm-certificates
# yum group install "Development Tools" -y
# yum install centos-release-openshift-origin39 #use correct version like 3.6 or 3.9
# yum install epel* [epel repository]
# sed -i -e "s/^enabled=1/enabled=0/" /etc/yum.repos.d/epel.repo #<====> to use epel repo only
# systemctl start NetworkManager
# systemctl enable NetworkManager
# yum install --enablerepo=epel install ansible pyOpenSSL #<===Only on master
```

## Step 3 : Clone the package from github

```
git clone https://github.com/OpenShift/OpenShift-ansible.git
cd OpenShift-ansible
git fetch
git checkout --track origin/release-3.9  #<====== Change branch to 3.9 release 
```

If you find issue check files in master branch itself.

## Step 4: Configure your DNS server

- [Configure  DNS](https://www.unixmen.com/setting-dns-server-centos-7/)
  
## Step 5 : Create a user with admin privilege

Grant full sudo access via sudoers file. eg:

```
# cat /etc/sudoers.d/ansible
ansible ALL=(ALL:ALL) NOPASSWD:ALL
```

## Step 6 : Start and Enable Docker

```
sudo systemctl stop docker
sudo systemctl restart docker
sudo systemctl enable docker
```

## Step 7 : Configure Storage

```
# cat /etc/sysconfig/docker-storage-setup
STORAGE_DRIVER=overlay2
DOCKER_ROOT_VOLUME=yes
DEVS=/dev/sdb
VG=vg_origin01
```

## Step 8 : Configure the inventory file

Amend the entries in inventory file

```
[OSEv3:children]
masters
nodes
etcd

[OSEv3:vars]
ansible_ssh_user=ansible
ansible_become=true
debug_level=2
deployment_type=origin
openshift_deployment_type=origin
openshift_release=v3.6
openshift_pkg_version=-3.6.0
openshift_image_tag=v3.6.0
openshift_service_catalog_image_version=v3.6.0
template_service_broker_image_version=v3.6.0
ansible_service_broker_install=False

#htpasswd auth
openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider', 'filename': '/etc/origin/master/htpasswd'}]
#openshift_master_identity_providers=[{'name': 'htpasswd_auth', 'login': 'true', 'challenge': 'true', 'kind': 'HTPasswdPasswordIdentityProvider'}]
openshift_master_default_subdomain=cloudapps.techbeatly.local

##containerized
containerized=true
openshift_disable_check=docker_image_availability,disk_availability,docker_storage,memory_availability
openshift_node_kubelet_args={'pods-per-core': ['10']}

# allow unencrypted connection within cluster
openshift_docker_insecure_registries=172.30.0.0/16
osm_use_cockpit=true
osm_etcd_image=registry.fedoraproject.org/f26/etcd
openshift_router_selector='region=primary'
openshift_registry_selector='region=primary'

# to fix the No package matching 'origin-docker-excluder-3.7.0 error
# Ref: https://github.com/openshift/origin/issues/17199#issuecomment-342494842
openshift_repos_enable_testing=true

openshift_ip=10.142.0.4

[masters]
ocp-master02 openshift_public_hostname=35.227.22.165 openshift_hostname="{{ inventory_hostname }}" openshift_ip=10.142.0.4 openshift_public_ip=35.227.22.165 #openshift_node_group_name=ocpgce

[etcd]
ocp-master02 #openshift_node_group_name=ocpgce

[nodes]
ocp-master02 openshift_node_labels="{'region': 'infra', 'zone': 'default'}" openshift_ip=10.142.0.4 openshift_public_ip=35.227.22.165 #openshift_node_group_name=ocpgce #openshift_schedulable=false
ocp-node03 openshift_node_labels="{'region': 'primary', 'zone': 'east'}" openshift_ip=10.142.0.5 openshift_public_ip=35.237.132.132 #openshift_node_group_name=ocpgce #openshift_schedulable=true
ocp-node04 openshift_node_labels="{'region': 'primary', 'zone': 'west'}" openshift_ip=10.142.0.8 openshift_public_ip=104.196.132.75 #openshift_node_group_name=ocpgce  #openshift_schedulable=true
```
## Step 9: Verify Pre-Req

Release 3.6, run below,

```
ansible-playbook playbooks/byo/openshift-preflight/check.yml -i inventory/ocp
```

If you are facing below error, 
```
fatal: [localhost]: FAILED! => {"attempts": 3, "changed": false, "msg": "No package matching 'origin-docker-excluder-3.11**' found available, installed or updated", "rc":
 126, "results": ["No package matching 'origin-docker-excluder-3.11**' found available, installed or updated"]} 
```

then use https://storage.googleapis.com/origin-ci-test/releases/openshift/origin/master/origin.repo.

##  Step 10: Deploy Cluster

If pre-check is successful then run the main deployment.

```
ansible-playbook playbooks/byo/config.yml -i inventory/ocp
```

If you face issues as below, refer [This](https://github.com/openshift/openshift-ansible/issues/7596) or [This](https://github.com/openshift/openshift-ansible/issues/7596#issuecomment-403356912) 

```
TASK [openshift_hosted_facts : Set hosted facts] *************************************************************************************************************************
task path: /home/ansible/openshift-ansible/roles/openshift_hosted_facts/tasks/main.yml:9
fatal: [ocp-master1]: FAILED! => {}
MSG:
|failed expects to merge two dicts
```

Also try by downgrading your ansible version. 

## Step 11: Configure cluster admin login

Once the cluster is up and online log on to the master node and setup a user and password.

```
[root@ocp-master1 ~]# htpasswd -b /etc/origin/master/htpasswd admin openshift
Updating password for user admin
[root@ocp-master1 ~]# openshift admin policy add-cluster-role-to-user cluster-admin admin
cluster role "cluster-admin" added: "admin"
```

## Step 12: Verify Cluster Status

From the master node verify the cluster is up with no errors by running the oc status command.

```
[root@ocp-master1 ~]# oc status
In project default on server https://10.142.0.2:8443
https://docker-registry-default.cloudapps.techbeatly.local (passthrough) (svc/docker-registry)
  dc/docker-registry deploys docker.io/openshift/origin-docker-registry:v3.6.0
	deployment #1 deployed 28 hours ago - 1 pod
svc/kubernetes - 172.30.0.1 ports 443->8443, 53->8053, 53->8053
https://registry-console-default.cloudapps.techbeatly.local (passthrough) (svc/registry-console)
  dc/registry-console deploys docker.io/cockpit/kubernetes:latest
	deployment #1 deployed 28 hours ago - 1 pod
svc/router - 172.30.96.77 ports 80, 443, 1936
  dc/router deploys docker.io/openshift/origin-haproxy-router:v3.6.0
	deployment #1 deployed 28 hours ago - 2 pods
View details with 'oc describe <resource>/<name>' or list everything with 'oc get all'.
 
 
[root@ocp-master1 ~]# oc get all
NAME                  DOCKER REPO                                                 TAGS  	UPDATED
is/registry-console   docker-registry.default.svc:5000/default/registry-console   latest    28 hours ago
NAME                  REVISION   DESIRED   CURRENT   TRIGGERED BY
dc/docker-registry    1      	1     	1     	config
dc/registry-console   1      	1     	1     	config
dc/router             1      	2     	2     	config
NAME                    DESIRED   CURRENT   READY     AGE
rc/docker-registry-1    1     	1     	1     	1d
rc/registry-console-1   1     	1     	1     	1d
rc/router-1             2     	2     	2     	1d
 
NAME                      HOST/PORT                                             PATH  	SERVICES       	POR
T  	TERMINATION   WILDCARD
routes/docker-registry    docker-registry-default.cloudapps.techbeatly.local          	docker-registry	<al
l>     passthrough   None
routes/registry-console   registry-console-default.cloudapps.techbeatly.local         	registry-console   <al
l>     passthrough   None
NAME                   CLUSTER-IP   	EXTERNAL-IP   PORT(S)               	AGE
svc/docker-registry    172.30.190.177   <none>    	5000/TCP              	1d
svc/kubernetes         172.30.0.1   	<none>    	443/TCP,53/UDP,53/TCP 	1d
svc/registry-console   172.30.212.204   <none>    	9000/TCP              	1d
svc/router	         172.30.96.77 	<none>    	80/TCP,443/TCP,1936/TCP   1d
NAME                          READY 	STATUS	RESTARTS   AGE
po/docker-registry-1-410r0    1/1   	Running   2          1d
po/registry-console-1-knq88   1/1   	Running   1          1d
po/router-1-6n4r7             1/1   	Running   1          1d
po/router-1-k65vw             1/1   	Running   2          1d
```

## Step 13: Verify basic pod status

Verify your metric pods are running without any errors.

```
[root@master ~]# oc get pods -n openshift-infra
```

## Step 14: Verify Web Console

Log on to your OpenShift Origin cluster via the browser using your master node as the URL with port 8443 - `https://master.dev.enron.com:8443`

**Note** : Make sure you have added firewall rules to allow traffic using 8443. (Both inside OS as well as on your infra like GCP/AWS or Openstack)

## Restart or Check Status of openshift service

```
[root@ocp-master02 ~]# systemctl status origin-master.service
[root@ocp-master02 ~]# systemctl restart origin-master.service
```

## Some more configs - Configuring Host Variables

To assign environment variables to hosts during the Ansible installation, indicate the desired variables in the /etc/ansible/hosts file after the host entry in the [masters] or [nodes] sections. 

For example:

```
[masters]
ec2-52-6-179-239.compute-1.amazonaws.com openshift_public_hostname=ose3-master.public.example.com
```

- `openshift_hostname` -  This variable overrides the internal cluster host name for the system. Use this when the system’s default IP address does not resolve to the system host name.
  
	`openshift_hostname=ose3-master.example.com`

- `openshift_public_hostname` - This variable overrides the system’s public host name. Use this for cloud installations, or for hosts on networks using a network address translation (NAT).
  
	`openshift_public_hostname=ose3-master.public.example.com`

- `openshift_ip` -  This variable overrides the cluster internal IP address for the system. Use this when using an interface that is not configured with the default route. This variable can also be used for etcd.

- `openshift_public_ip` - This variable overrides the system’s public IP address. Use this for cloud installations, or for hosts on networks using a network address translation (NAT).
  
- `openshift_master_cluster_hostname` - This variable overrides the host name for the cluster, which defaults to the host name of the master.

  `openshift_master_cluster_hostname=openshift-internal.example.com`

- `openshift_master_cluster_public_hostname` - This variable overrides the public host name for the cluster, which defaults to the host name of the master.

	`openshift_master_cluster_public_hostname=openshift-cluster.example.com`

Refer : [Advanced Installation](https://docs.openshift.com/container-platform/3.5/install_config/install/advanced_install.html)

## Verify PublicURL

```
[root@ocp-master02 ~]# grep PublicURL /etc/origin/master/master-config.yaml
  masterPublicURL: https://35.227.22.165:8443
masterPublicURL: https://35.227.22.165:8443
  assetPublicURL: https://35.227.22.165:8443/console/
  masterPublicURL: https://35.227.22.165:8443
```


## Uninstalling OCP Cluster    

If you need to uninstall the cluster and start over. Run the following command.

```
ansible-playbook -i ./inventory/byo/hosts playbooks/adhoc/uninstall.yml
```

Deleting all your pods and services in your project in order to start over.

```
oc delete all  --all -n test01
```
 
# Appendix

## References

- https://www.server-world.info/en/note?os=CentOS_7&p=OpenShift37 
- https://www.projectatomic.io/blog/2016/12/part1-install-origin-on-f25-atomic-host/ 
- https://gauvain.pocentek.net/openshift-single-node.html
    
debug command: `curl -L -k https://<your ip>:8443/console`s
