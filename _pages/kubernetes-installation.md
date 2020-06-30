---
layout: post
title: Kubernetes Installation
#author: gini
categories: [ kubernetes ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: /kubernetes-installation
featured: false
hidden: false
---

<!-- TOC orderedlist:true -->

- [2. minikube](#2-minikube)
  - [2.1. Just to test ? Use this Lab](#21-just-to-test--use-this-lab)
  - [2.2. Download and Install minikube](#22-download-and-install-minikube)
  - [2.3. Start minikube](#23-start-minikube)
    - [2.3.1. Start minikube cluster](#231-start-minikube-cluster)
  - [2.4. Install kubectl](#24-install-kubectl)
  - [2.5. Kubernetes Dashboard](#25-kubernetes-dashboard)
- [3. kubernetes-installation-using-kubeadm](#3-kubernetes-installation-using-kubeadm)
- [4. Installing Kubernetes the Hardway](#4-installing-kubernetes-the-hardway)
  - [4.1. Install virtualbox and vagarnt](#41-install-virtualbox-and-vagarnt)
  - [4.2. then....](#42-then)
- [Kubernetes Installation using Ansible](#kubernetes-installation-using-ansible)

<!-- /TOC -->

# 2. minikube

## 2.1. Just to test ? Use this Lab

There is a simple lab to see and test minikube; use it **[here](https://kubernetes.io/docs/tutorials/hello-minikube/#create-a-minikube-cluster)**

Ref: 
- https://github.com/kubernetes/minikube
- https://minikube.sigs.k8s.io/docs/start/linux/

*Note : Below steps are based on debian based, you may refer document for other platforms.*

## 2.2. Download and Install minikube

```
# download and install package
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_1.5.2.deb \
 && sudo dpkg -i minikube_1.5.2.deb

# check version
minikube version
```

## 2.3. Start minikube 

minikube will need a hypervisor (VirtualBox or KVM) but if you are already inside a virtual machine (and nested virtualization is not possible), then it is possible to skip the creation of an additional VM layer by using the none driver. But please note, you need to install and setup docker environment in that vm.(ref **[Install Docker on Debian](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-9)**)

To make none the default driver:

```
sudo minikube config set vm-driver none
```

You can also mention the kubernetes version you need to install.

```
minikube start --kubernetes-version v1.16.0
```

### 2.3.1. Start minikube cluster

```
$ sudo minikube start --vm-driver=none --wait=false
😄  minikube v1.5.2 on Debian 9.11
🤹  Running on localhost (CPUs=2, Memory=7483MB, Disk=10013MB) ...
ℹ️   OS release is Debian GNU/Linux 9 (stretch)
⚠️  VM may be unable to resolve external DNS records
🐳  Preparing Kubernetes v1.16.2 on Docker '19.03.5' ...
💾  Downloading kubelet v1.16.2
💾  Downloading kubeadm v1.16.2
🚜  Pulling images ...
🚀  Launching Kubernetes ... 
🤹  Configuring local host environment ...

⚠️  The 'none' driver provides limited isolation and may reduce system security and reliability.
⚠️  For more information, see:
👉  https://minikube.sigs.k8s.io/docs/reference/drivers/none/

⚠️  kubectl and minikube configuration will be stored in /root
⚠️  To use kubectl or minikube commands as your own user, you may need to relocate them. For example, to overwrite your own settings, run:

    ▪ sudo mv /root/.kube /root/.minikube $HOME
    ▪ sudo chown -R $USER $HOME/.kube $HOME/.minikube

💡  This can also be done automatically by setting the env var CHANGE_MINIKUBE_NONE_USER=true
⌛  Waiting for: apiserver
🏄  Done! kubectl is now configured to use "minikube"
💡  For best results, install kubectl: https://kubernetes.io/docs/tasks/tools/install-kubectl/

```

Now we have a running kubernetes cluster and let us see the status.

## 2.4. Install kubectl

You need `kubectl` to manage your cluster resources.
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl 

# Make the kubectl binary executable.
chmod +x ./kubectl

# Move the binary in to your PATH.
sudo mv ./kubectl /usr/local/bin/kubectl

# Test to ensure the version you installed is up-to-date:
kubectl version
```

Now we switch to root or use 
Verify cluster inf
```
kubectl cluster-info

kubectl get nodes
```

## 2.5. Kubernetes Dashboard

You can enable the dashboard as below. (Take another console to run it)

```
$ sudo minikube dashboard
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
http://127.0.0.1:39067/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/

```

Access the url and check. 

# 3. kubernetes-installation-using-kubeadm

- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#before-you-begin
- https://medium.com/containerum/4-ways-to-bootstrap-a-kubernetes-cluster-de0d5150a1e4

Upgrade the version of kubeadm on the master and node01 to 1.16.0
For easier installation, use apt-get install instead of upgrade

```
sudo apt-get update && sudo apt-get install -qy kubeadm=1.16.0-00
```

Initialize Control Plane Node (Master)

```
$ kubeadm init

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.17.0.17:6443 --token baawxx.9ez921jerna2rb6f \
    --discovery-token-ca-cert-hash sha256:98cb7e30b0361da3e3eb689cd74af9288dfcc99d36c9b5541b1765f9a9f0bd20
```

If you do not have the token, you can get it by running the following command on the control-plane node:

```
kubeadm token list
```

By default, tokens expire after 24 hours. If you are joining a node to the cluster after the current token has expired, you can create a new token by running the following command on the control-plane node:

on node01
```
kubeadm join 172.17.0.17:6443 --token baawxx.9ez921jerna2rb6f \
    --discovery-token-ca-cert-hash sha256:98cb7e30b0361da3e3eb689cd74af9288dfcc99d36c9b5541b1765f9a9f0bd20
```

Install a Network Plugin. As a default, we will go with weave

```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

# 4. Installing Kubernetes the Hardway

- [GCP](https://github.com/ginigangadharan/kubernetes-the-hard-way-gcp)
- [VirtualBox](https://github.com/ginigangadharan/kubernetes-the-hard-way-virtualbox)

## 4.1. Install virtualbox and vagarnt

## 4.2. then....

# Kubernetes Installation using Ansible

- https://www.youtube.com/watch?v=kMKkpgB6IwM
- https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/
- https://medium.com/faun/how-to-create-your-own-kubernetes-cluster-using-ansible-7c6b5c031a5d

