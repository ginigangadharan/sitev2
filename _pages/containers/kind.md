---
layout: post
title: kind - Kubernetes in Docker
author: gini
categories: [ kubernets ]
image: "assets/images/2021/boat-20427_1920-kubernetes-helm.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: kind
featured: false
hidden: false
titleshort: kind
---

- [Installating kind](#installating-kind)
  - [Installating kind on Linux](#installating-kind-on-linux)
  - [Create a kind cluster](#create-a-kind-cluster)
  - [Create kind cluster using config file.](#create-kind-cluster-using-config-file)
- [With ingress](#with-ingress)
- [Installing kind on MacOS](#installing-kind-on-macos)
- [How to login to a "node" in kind cluster](#how-to-login-to-a-node-in-kind-cluster)
- [Removing clusters](#removing-clusters)
- [References](#references)
- [Troubleshooting](#troubleshooting)


## Installating kind

### Installating kind on Linux

**Install Docker**

```shell
## remove if any docker components
sudo apt-get remove docker docker-engine docker.io containerd runc

## Update packages
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

## Add Docker’s official GPG key:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

## set repo
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Install docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

## Add user to docker group
sudo usermod -a -G docker $USER
```

**Install Kind**

```shell
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
chmod +x ./kind
## move to any system path if needed
sudo mv ./kind /usr/bin/kind
```

### Create a kind cluster

`kind create cluster` will create a cluster with below specs.

- Cluster name will be ‘kind’
- Cluster will have only one node (control plane node only)


```shell
kind create cluster --name test-kind
```

**Multi-node clusters**

Check [kind-example-config](https://raw.githubusercontent.com/kubernetes-sigs/kind/main/site/content/docs/user/kind-example-config.yaml) sample.

```yaml
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

```yaml
# a cluster with 3 control-plane nodes and 3 workers
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: control-plane
- role: control-plane
- role: worker
- role: worker
- role: worker
```

Sample kind configuration file

```yaml
# this config file contains all config fields with comments
# NOTE: this is not a particularly useful config file
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: kind-demo-multinode
networking:
  # WARNING: It is _strongly_ recommended that you keep this the default
  # (127.0.0.1) for security reasons. However it is possible to change this.
  apiServerAddress: "127.0.0.1"
  # By default the API server listens on a random open port.
  # You may choose a specific port but probably don't need to in most cases.
  # Using a random port makes it easier to spin up multiple clusters.
  apiServerPort: 6443
# patch the generated kubeadm config with some extra settings
kubeadmConfigPatches:
- |
  apiVersion: kubelet.config.k8s.io/v1beta1
  kind: KubeletConfiguration
  evictionHard:
    nodefs.available: "0%"
# patch it further using a JSON 6902 patch
kubeadmConfigPatchesJSON6902:
- group: kubeadm.k8s.io
  version: v1beta2
  kind: ClusterConfiguration
  patch: |
    - op: add
      path: /apiServer/certSANs/-
      value: my-hostname
# 1 control plane node and 3 workers
nodes:
# the control plane node config
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
# the three workers
- role: worker
- role: worker
```

### Create kind cluster using config file.

```shell
kind create cluster --config kind-example-config.yaml

$ kubectl get nodes
NAME                 STATUS   ROLES                  AGE     VERSION
kind-control-plane   Ready    control-plane,master   8m20s   v1.21.1
kind-worker          Ready    <none>                 7m49s   v1.21.1
kind-worker2         Ready    <none>                 7m48s   v1.21.1

$ kubectl cluster-info --context kind-kind
Kubernetes control plane is running at https://127.0.0.1:6443
CoreDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

## With ingress

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: demo
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"    
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
```

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

## Installing kind on MacOS

```shell
## Install on mac
brew install kind
```

```shell
## Start using podman
KIND_EXPERIMENTAL_PROVIDER=podman kind create cluster
```

## How to login to a "node" in kind cluster

```shell
## get docker container details
docker ps
docker exec -it CONTAINER_ID /bin/sh
```

## Removing clusters

```shell
kind delete cluster
kind delete cluster --name test-k8s
```


## References

- [Kubernetes with kind](https://www.baeldung.com/ops/kubernetes-kind) - baeldung.com


## Troubleshooting

```shell
gini@greenmango ginigangadharan.github.io % kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.21.1) 🖼
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
ERROR: failed to create cluster: failed to get kubeconfig to merge: yaml: unmarshal errors:
  line 47: mapping key "apiVersion" already defined at line 1
  line 48: mapping key "clusters" already defined at line 2
  line 53: mapping key "contexts" already defined at line 11
  line 58: mapping key "current-context" already defined at line 36
  line 59: mapping key "kind" already defined at line 37
  line 60: mapping key "preferences" already defined at line 38
  line 61: mapping key "users" already defined at line 39
```