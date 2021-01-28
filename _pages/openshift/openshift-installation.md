---
layout: post
title: OpenShift Installation Methods - Examples
author: gini
categories: [ openshift ]
tags: [ OpenShift Installation ]
show-avatar: false
permalink: /openshift-installation
featured: false
hidden: false
titleshort: OpenShift Installation
---

- [1. Installing an OKD 4.x Cluster](#1-installing-an-okd-4x-cluster)
- [2. Install OpenShift 4.x](#2-install-openshift-4x)
- [3. Method I - Setup an OpenShift All-In-One](#3-method-i---setup-an-openshift-all-in-one)
  - [3.1. Install required packages](#31-install-required-packages)
  - [3.2. Disable Firewall](#32-disable-firewall)
  - [3.3. Installing OpenShift CLI](#33-installing-openshift-cli)
    - [3.3.1. Method 1 - Standard CentOS repositories](#331-method-1---standard-centos-repositories)
    - [3.3.2. Method 2 - Download and extract openshift origin](#332-method-2---download-and-extract-openshift-origin)
      - [3.3.2.1. Add the directory you untarred the release into to your path:](#3321-add-the-directory-you-untarred-the-release-into-to-your-path)
  - [3.4. Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16](#34-configure-the-docker-daemon-with-an-insecure-registry-parameter-of-172300016)
    - [3.4.1. Restart docker service](#341-restart-docker-service)
  - [3.5. Initiate cluster](#35-initiate-cluster)
    - [3.5.1. Ref:](#351-ref)
    - [3.5.2. Add a user](#352-add-a-user)
- [4. Method II - Setup minishift](#4-method-ii---setup-minishift)
  - [4.1. Setup Virtual Environment](#41-setup-virtual-environment)
  - [4.2. Install minishift](#42-install-minishift)
  - [4.3. Start minishift cluster](#43-start-minishift-cluster)
- [5. Method III - OpenShift 4 - All in One Quick Cluster](#5-method-iii---openshift-4---all-in-one-quick-cluster)
- [6. Method IV - OpenShift Full Cluster](#6-method-iv---openshift-full-cluster)
- [7. CodeReady Containers - CRC (OpenShift 4.x)](#7-codeready-containers---crc-openshift-4x)
  - [7.1. Download Package](#71-download-package)
  - [7.2. Required software packages](#72-required-software-packages)
    - [7.2.1. Enable sudo for user](#721-enable-sudo-for-user)
  - [7.3. Setup Cluster](#73-setup-cluster)
  - [7.4. Access your Cluster](#74-access-your-cluster)
  - [7.5. Troubleshooting](#75-troubleshooting)
    - [7.5.1. After crc start and crc console, oc login fails with Internal error occurred: unexpected response: 503 for a while #740](#751-after-crc-start-and-crc-console-oc-login-fails-with-internal-error-occurred-unexpected-response-503-for-a-while-740)
    - [7.5.2. Unable to connect to console](#752-unable-to-connect-to-console)
- [8. OpenSHift 4.x](#8-openshift-4x)
- [9. OpenShift 4.2 Installation](#9-openshift-42-installation)
- [10. Red Hat OpenShift 4.x Installation (Evaluation)](#10-red-hat-openshift-4x-installation-evaluation)
  - [10.1. Baremetal Installation](#101-baremetal-installation)
- [11. OpenShift on Baremetal (UPI)](#11-openshift-on-baremetal-upi)
- [12. OpenShift All-In-One - OKD Using Ansible](#12-openshift-all-in-one---okd-using-ansible)
- [13. Extras OpenSHift 4.x](#13-extras-openshift-4x)
  - [13.1. OpenShift 4.2 Installation](#131-openshift-42-installation)
  - [13.2. OpenShift 4.1 Installation](#132-openshift-41-installation)
  - [13.3. Baremetal Installation](#133-baremetal-installation)
- [14. baremetal](#14-baremetal)
- [15. Deploy OpenShift Using OpenShift Installer (AWS)](#15-deploy-openshift-using-openshift-installer-aws)
  - [15.1. Using OpenShift Installer](#151-using-openshift-installer)
  - [Setup Bastion Host](#setup-bastion-host)
    - [Configure Bastion VM to Run OpenShift Installer](#configure-bastion-vm-to-run-openshift-installer)
    - [Install OpenShift Container Platform](#install-openshift-container-platform)
    - [multi-step installation.](#multi-step-installation)
    - [Clean Up Cluster](#clean-up-cluster)
    - [Validate Cluster](#validate-cluster)
  - [References](#references)

# 1. Installing an OKD 4.x Cluster

[Reference](https://www.openshift.com/blog/guide-to-installing-an-okd-4-4-cluster-on-your-home-lab)
[Installing an OKD 4.5 Cluster](https://medium.com/@craig_robinson/guide-installing-an-okd-4-5-cluster-508a2631cbee)

# 2. Install OpenShift 4.x

[How to install OpenShift 4 on Bare Metal - (UPI)](https://www.youtube.com/watch?v=d03xg2PKOPg) (Video)

# 3. Method I - Setup an OpenShift All-In-One 

## 3.1. Install required packages
```
yum install ansible docker wget -y
systemctl enable docker
systemctl start docker
```

## 3.2. Disable Firewall
Or you have to open required ports.
```
systemctl disable firewalld
systemctl stop firewalld
```

## 3.3. Installing OpenShift CLI

### 3.3.1. Method 1 - Standard CentOS repositories
```
yum -y install centos-release-openshift-origin39
yum -y install origin-clients
```

### 3.3.2. Method 2 - Download and extract openshift origin

Create a directory for data (anywhere)
```
mkdir /data
cd /data
```

Check the latest version if you need.
```
wget "https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz"
tar -xzvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz 
cd openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit
```

#### 3.3.2.1. Add the directory you untarred the release into to your path:
```
export PATH=$PATH:/data/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit/

# or

export PATH=$PATH:`pwd`
```

## 3.4. Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16
```
cat > /etc/docker/daemon.json <<DELIM
{
   "insecure-registries": [
     "172.30.0.0/16"
   ]
}
DELIM
```

### 3.4.1. Restart docker service
```
service docker restart
```

## 3.5. Initiate cluster
```
oc cluster up --base-dir="/data/clusterup" --public-hostname=<IP>
```
- `--base-dir=BASE_DIR` : Directory on Docker host for cluster up configuration

(oc cluster up --public-hostname=35.239.51.76 --routing-suffix=35.239.51.76.xip.io)
openshift.local.clusterup/openshift-controller-manager/openshift-master.kubeconfig


### 3.5.1. Ref:
- https://github.com/openshift/origin/blob/release-3.11/docs/cluster_up_down.md
- https://medium.com/@fabiojose/working-with-oc-cluster-up-a052339ea219


### 3.5.2. Add a user
```
# oc create user redhat
user.user.openshift.io/redhat created
# oc adm policy add-cluster-role-to-user cluster-admin redhat
cluster role "cluster-admin" added: "redhat"
```

# 4. Method II - Setup minishift

## 4.1. Setup Virtual Environment

- setup KVM or virtualbox (or other virtulization)
Ref: [Set up your virtualization environment](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html)

## 4.2. Install minishift
- [Download](https://github.com/minishift/minishift/releases) and manually install 

or 

Installation on mac
```
brew cask install minishift
```

* in case issue to install `export HOMEBREW_NO_ENV_FILTERING=1`*

## 4.3. Start minishift cluster

```
minishift start --vm-driver virtualbox

# setup VirtualBox Permanently
minishift config set vm-driver virtualbox
```

Once started, access the console using url or open in browser
```
minishift console
```

setup oc access
```
$ minishift oc-env
export PATH="/home/john/.minishift/cache/oc/v1.5.0:$PATH"
# Run this command to configure your shell:
# eval $(minishift oc-env)
```

# 5. Method III - OpenShift 4 - All in One Quick Cluster

https://github.com/openshift/okd/releases

# 6. Method IV - OpenShift Full Cluster

- [Installing OpenShift 4.1 Using Libvirt and KVM](http://alesnosek.com/blog/2019/07/08/installing-openshift-4-dot-1-using-libvirt-and-kvm/)

# 7. CodeReady Containers - CRC (OpenShift 4.x)

[Red Hat CodeReady Containers](https://developers.redhat.com/products/codeready-containers)
  
Ref: 
- [CodeRead Containers - Red Hat OpenShift 4 on your laptop](https://developers.redhat.com/blog/2019/09/05/red-hat-openshift-4-on-your-laptop-introducing-red-hat-codeready-containers/)
- [Install OpenShift Container Platform 4](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)
- [CodeReady Containers on Ubuntu](https://labs.consol.de/devops/linux/2019/11/29/codeready-containers-on-ubuntu.html)
- [Trying out CRC - Jeff](https://www.jeffgeerling.com/blog/2019/trying-out-crc-code-ready-containers-run-openshift-4x-locally)
- [RED HAT CODEREADY CONTAINERS](https://access.redhat.com/documentation/en-us/red_hat_codeready_containers/1.13/html-single/getting_started_guide/index)


## 7.1. Download Package

Refer : **[Install OpenShift on a laptop with CodeReady Containers](https://cloud.redhat.com/openshift/install/crc/installer-provisioned?intcmp=7013a000002CtetAAC)**

- Visit [https://www.openshift.com/try](https://www.openshift.com/try)
- Choose install on Laptop -> https://cloud.redhat.com/openshift/install/crc/installer-provisioned
- Download for your OS choise (Windows10, MacOS, Linux)
- Move package to your machine folder
- extract the package 
  `tar -xf FILENAME.tar.xz`

- Download and keep the pull secret from same location. You need that later during `crc start`

## 7.2. Required software packages

CodeReady Containers requires the libvirt and NetworkManager packages. 
```shell
yum install NetworkManager
wget https://mirror.openshift.com/pub/openshift-v4/clients/crc/latest/crc-linux-amd64.tar.xz
```

### 7.2.1. Enable sudo for user

```
[devops@node1 ~]$ sudo cat /etc/sudoers.d/devops
[sudo] password for devops:
devops ALL=(ALL) NOPASSWD: ALL
```

## 7.3. Setup Cluster

```bash
$ cd crc-linux-1.11.0-amd64
$ export PATH=/home/devops/crc-linux-1.11.0-amd64:$PATH

## set up your host operating system for the CodeReady Containers virtual machine.
## Use normal user account
$ crc setup

## Start cluster
## if you are on a terminal without GUI, you will have difficulty to copy/paste pull secret content. in that you can mention the pull secret ful
$ crc start -p /path-to/pull-secret

```

## 7.4. Access your Cluster
```
$ eval $(crc oc-env)
$ oc login -u developer -p developer

## see credentials
$ crc console --credentials
To login as a regular user, run 'oc login -u developer -p developer https://api.crc.testing:6443'.
To login as an admin, run 'oc login -u kubeadmin -p 8rynV-SeYLc-h8Ij7-YPYcz https://api.crc.testing:6443'

## Access Console
$ crc console
Opening the OpenShift Web Console in the default browser...

```

## 7.5. Troubleshooting

### 7.5.1. After crc start and crc console, oc login fails with Internal error occurred: unexpected response: 503 for a while #740

### 7.5.2. Unable to connect to console

```
$ crc ip
$ nmcli conn show

```

# 8. OpenSHift 4.x 

https://github.com/openshift/okd

Create `clouds.yaml`
```
clouds:
  ocp4-dev:
    auth:
      auth_url: http://10.6.1.209:35357/
      project_name: ocp4-dev
      username: ocpadmin
      password: ocpadmin
    region_name: RegionOne
``` 
# 9. OpenShift 4.2 Installation
https://docs.openshift.com/container-platform/4.2/installing/installing_bare_metal/installing-bare-metal.html

# 10. Red Hat OpenShift 4.x Installation (Evaluation)
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/index

- [Get Evaluation](https://www.redhat.com/en/technologies/cloud-computing/openshift/try-it/success)
- [Installation](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.5/html/architecture/architecture-installation#installation-overview_architecture-installation)

## 10.1. Baremetal Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/installing-on-bare-metal
https://docs.openshift.com/container-platform/4.1/installing/installing_bare_metal/installing-bare-metal.html
https://blog.openshift.com/openshift-4-bare-metal-install-quickstart/


# 11. OpenShift on Baremetal (UPI)

- [Installing on OpenShift on Baremetal](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.4/html/installing_on_bare_metal/index)

# 12. OpenShift All-In-One - OKD Using Ansible

Ref:
https://github.com/Gepardec/ansible-role-okd
https://galaxy.ansible.com/gepardec/okd
https://computingforgeeks.com/setup-openshift-origin-local-cluster-on-centos/

# 13. Extras OpenSHift 4.x 

https://github.com/openshift/okd

Create `clouds.yaml`
```
clouds:
  ocp4-dev:
    auth:
      auth_url: http://10.6.1.209:35357/
      project_name: ocp4-dev
      username: ocpadmin
      password: ocpadmin
    region_name: RegionOne
``` 

## 13.1. OpenShift 4.2 Installation
https://docs.openshift.com/container-platform/4.2/installing/installing_bare_metal/installing-bare-metal.html

## 13.2. OpenShift 4.1 Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/index

## 13.3. Baremetal Installation
https://access.redhat.com/documentation/en-us/openshift_container_platform/4.1/html/installing/installing-on-bare-metal
https://docs.openshift.com/container-platform/4.1/installing/installing_bare_metal/installing-bare-metal.html
https://blog.openshift.com/openshift-4-bare-metal-install-quickstart/

https://blog.openshift.com/revamped-openshift-all-in-one-aio-for-labs-and-fun/

VMware
https://labs.consol.de/container/platform/openshift/2020/01/31/ocp43-installation-vmware.html

https://blogs.ovirt.org/2019/01/ovirt-openshift-part-1/

# 14. baremetal
https://docs.openshift.com/container-platform/4.3/installing/installing_bare_metal/installing-bare-metal.html#cluster-entitlements_installing-bare-metal

# 15. Deploy OpenShift Using OpenShift Installer (AWS)
(27 Jan 2021)
- [OpenShift Installer](https://www.openshift.com/try)
  
Installer configures entire cloud infrastructure:
- VMs
- Load balancers
- Storage
- Networking
- Other resources

## 15.1. Using OpenShift Installer

```
openshift-install create cluster --dir=$HOME/mycluster
```
Installer prompts for
- SSH public key
- Platform: aws
- Region: Default in AWS is us-east-1
- Base domain: Public route 53 domain, needs to exist prior to installation
- Cluster name: Must be unique within AWS account
- Pull secret: From Get Started with OpenShift as single-line JSON

## Setup Bastion Host
- Create a bastion node and login to the host.

```
$ ssh user@bastion-host
$ echo $GUID
d0ce
```

### Configure Bastion VM to Run OpenShift Installer

**Install AWS CLI**

```
# Download the latest AWS Command Line Interface
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip

# Install the AWS CLI into /bin/aws
./awscli-bundle/install -i /usr/local/aws -b /bin/aws

# Validate that the AWS CLI works
aws --version

# Clean up downloaded files
rm -rf /root/awscli-bundle /root/awscli-bundle.zip
```

**Download OpenShift Installation Binary**
```
OCP_VERSION=4.6.4
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/${OCP_VERSION}/openshift-install-linux-${OCP_VERSION}.tar.gz
tar zxvf openshift-install-linux-${OCP_VERSION}.tar.gz -C /usr/bin
rm -f openshift-install-linux-${OCP_VERSION}.tar.gz /usr/bin/README.md
chmod +x /usr/bin/openshift-install
```

**Download and Install OC CLI**
```
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/${OCP_VERSION}/openshift-client-linux-${OCP_VERSION}.tar.gz
tar zxvf openshift-client-linux-${OCP_VERSION}.tar.gz -C /usr/bin
rm -f openshift-client-linux-${OCP_VERSION}.tar.gz /usr/bin/README.md
chmod +x /usr/bin/oc
```

- Check that the OpenShift installer and CLI are in /usr/bin:

```bash
ls -l /usr/bin/{oc,openshift-install}

# Setup auto-completion
oc completion bash >/etc/bash_completion.d/openshift
```

- logout from `root`

**Configure AWS CLI Credential**
```bash
export AWSKEY=<YOURACCESSKEY>
export AWSSECRETKEY=<YOURSECRETKEY>
export REGION=us-east-2

mkdir $HOME/.aws
cat << EOF >>  $HOME/.aws/credentials
[default]
aws_access_key_id = ${AWSKEY}
aws_secret_access_key = ${AWSSECRETKEY}
region = $REGION
EOF

# test AWS Access
aws sts get-caller-identity
```

- Open https://www.openshift.com/try and select "Try it in the Cloud", Select AWS
- Choose "Installer-Provisioned-Infrastructure"
- Copy the Pull Secret (save to file for copy)

- Create an ssh key pair

```
ssh-keygen -f ~/.ssh/cluster-${GUID}-key -N ''
```

### Install OpenShift Container Platform

- Installer will generate Ignition configs for the bootstrap, master, and worker machines. 
- The process for bootstrapping a cluster looks like the following:
  - The bootstrap machine boots and starts hosting the remote resources required for the master machines to boot.
  - The master machines fetch the remote resources from the bootstrap machine and finish booting.
  - The master machines use the bootstrap node to form an etcd cluster.
  - The bootstrap node starts a temporary Kubernetes control plane using the newly created etcd cluster.
  - The temporary control plane schedules the production control plane to the master machines.
  - The temporary control plane shuts down, yielding to the production control plane.
  - The bootstrap node injects OpenShift-specific components into the newly formed control plane.
  - The installer then tears down the bootstrap node.

- The result of this bootstrapping process is a fully running OpenShift cluster. The cluster will then download and configure the remaining components needed for day-to-day operation, including the creation of worker machines on supported platforms.

**Run OpenShift Installer**

Run the OpenShift installer and answer the prompts:
- Select your Public Key (which you have created earlier)
- Select aws as the Platform.
- Select any Region near you.
- Select `cluster.yourdomain.com` as the Base Domain.
- For the Cluster Name, type cluster-101 (or any other name)
- When prompted, paste the contents of your Pull Secret in JSON format. Do not include any spaces or white characters and make sure it is in one line

```bash
$ openshift-install create cluster --dir $HOME/cluster-101

# Sample answers
? SSH Public Key /home/user/.ssh/cluster-101-key.pub
? Platform aws
INFO Credentials loaded from the "default" profile in file "/home/user/.aws/credentials"
? Region us-east-2 (Ohio)
? Base Domain cluster.yourdomain.com
? Cluster Name cluster-101
? Pull Secret [? for help] ***************************************************************************************************************************************************************

# wait for installer to finish

*********************************************
INFO Creating infrastructure resources...
INFO Waiting up to 20m0s for the Kubernetes API at https://api.cluster-d0ce.d0ce.sandbox1072.opentlc.com:6443... 
INFO API v1.19.0+9f84db3 up
INFO Waiting up to 30m0s for bootstrapping to complete...
INFO Destroying the bootstrap resources...        
INFO Waiting up to 40m0s for the cluster at https://api.cluster-d0ce.d0ce.sandbox1072.opentlc.com:6443 to initialize... 
INFO Waiting up to 10m0s for the openshift-console route to be created... 
INFO Install complete!
INFO To access the cluster as the system:admin user when using 'oc', run 'export KUBECONFIG=/home/gineesh.madapparambath-fujitsu.c/cluster-d0ce/auth/kubeconfig'
INFO Access the OpenShift web-console here: https://console-openshift-console.apps.cluster-d0ce.d0ce.sandbox1072.opentlc.com
INFO Login to the console with user: "kubeadmin", and password: "6TVYn-33gLY-7ZjrA-eqRQE" 
INFO Time elapsed: 37m56s
```

- Make note of the following items from the output of the install command:
  - The location of the kubeconfig file, which is required for setting the KUBECONFIG environment variable and, as suggested, sets the OpenShift user ID to `system:admin`.
  - The kubeadmin user ID and associated password (GEveR-tBVTB-jJUJB-iC9Jn in the example).
  - The password for the kubeadmin user is also written into the auth/kubeadmin-password file.
  - The URL of the web console - (https://console-openshift-console.apps.cluster-<GUID>.sandbox<SANDBOXID>.opentlc.com in the example) and the credentials (again) to log into the web console.

- Refer `${HOME}/cluster-101/.openshift_install.log` for logs and troubleshooting.



### multi-step installation.

```bash
# Create the installation configuration: 
openshift-install create install-config --dir $HOME/cluster-${GUID}.

# Update the generated install-config.yaml file—for example, change the AWS EC2 instance types.

# Create the YAML manifest templates: 
openshift-install create manifests --dir $HOME/cluster-${GUID}.
#  Changing the manifest templates is unsupported.

# Create the YAML manifests:
openshift-install create manifests --dir $HOME/cluster-${GUID}
#  Changing the manifests is unsupported.

# Create the Ignition configuration files: 
openshift-install create ignition-configs --dir $HOME/cluster-${GUID}.
# Changing the Ignition configuration files is unsupported.

# Install the cluster: 
openshift-install create cluster --dir $HOME/cluster-${GUID}.

# To delete the cluster, use: 
openshift-install destroy cluster --dir $HOME/cluster-${GUID}.

```

### Clean Up Cluster

```bash
openshift-install destroy cluster --dir $HOME/cluster-${GUID}

# Delete all of the files created by the OpenShift installer:
rm -rf $HOME/.kube
rm -rf $HOME/cluster-${GUID}
```

### Validate Cluster

```bash
# Setup CLI
export KUBECONFIG=$HOME/cluster-${GUID}/auth/kubeconfig
echo "export KUBECONFIG=$HOME/cluster-${GUID}/auth/kubeconfig" >>$HOME/.bashrc

$ oc whoami
system:admin

# get console and login with kubeadmin & password from installation log
$ oc whoami --show-console
https://console-openshift-console.apps.cluster-d0ce.d0ce.sandbox1072.opentlc.com


```
## References
- [Installing a cluster quickly on AWS](https://docs.openshift.com/container-platform/4.6/installing/installing_aws/installing-aws-default.html)
- [OpenShift Installer overview on GitHub](https://github.com/openshift/installer/blob/master/docs/user/overview.md)

