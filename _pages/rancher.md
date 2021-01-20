

- [1. Introduction to Rancher & RKE](#1-introduction-to-rancher--rke)
  - [1.1. Rancher Architecture](#11-rancher-architecture)
  - [1.2. Disocvering RKE](#12-disocvering-rke)
    - [1.2.1. Node Preparation](#121-node-preparation)
    - [1.2.2. Creating the Cluster Configuration File](#122-creating-the-cluster-configuration-file)
    - [1.2.3. Certificate Options](#123-certificate-options)
  - [1.3. Day Two Operations For RKE](#13-day-two-operations-for-rke)
    - [1.3.1. Secure the Installation Files](#131-secure-the-installation-files)
    - [1.3.2. Backups and DR](#132-backups-and-dr)
    - [1.3.3. Upgrade an RKE Cluster](#133-upgrade-an-rke-cluster)
    - [1.3.4. Certificate Management](#134-certificate-management)
    - [1.3.5. Adding and Removing Nodes](#135-adding-and-removing-nodes)
- [2. Installing Rancher](#2-installing-rancher)
- [3. References](#3-references)


# 1. Introduction to Rancher & RKE

## 1.1. Rancher Architecture

## 1.2. Disocvering RKE

- RKE deploys Kubernetes components as docker containers
- SSH is used to orchestrate RKE across servers
- Cluster.yml contains all the information RKE needs to provision a Kubernetes cluster
- You can supply your own certificates that Rancher will serve for its UI/API
- RKE will support the Latest patch releases from the three most recent minor releases correct

### 1.2.1. Node Preparation

[Installation Requirements](https://rancher.com/docs/rancher/v2.x/en/installation/requirements/)

- SSH user in docker group
- Disable sawp on workers

Install docker
`curl https://releases.rancher.com/install-docker/19.03.sh | sh`

- Download and install RKE from https://github.com/rancher/rke/releases
- Update path.

```
~$ rke --version
rke version v1.0.6
```

### 1.2.2. Creating the Cluster Configuration File
```
$ rke config
```
Answer questions and create `cluster.yaml`

```
$ rke up --ssh-agent-auth
```

[Creating the Cluster Configuration File](https://rancher.com/docs/rke/latest/en/installation/#using-rke-config)

### 1.2.3. Certificate Options

## 1.3. Day Two Operations For RKE

### 1.3.1. Secure the Installation Files

Save `kube_config_cluster.yaml` and `cluster.rkestate`

### 1.3.2. Backups and DR

- Snapshot every 6 hours and keep it for 24 hours
- or take manual snaphort `rke etcd snapshort-save`, which will save snapshot in `/opt/rke/etcd-snapshots/` directory
- minio to keep snapshots
- `rke etcd snapshot-restore --name BACKUP_FILE` to restore

### 1.3.3. Upgrade an RKE Cluster

- Use `rke config` to list the versions of Kubernetes supported by this version of rke
- modify config to new version - [Ref](https://rancher.com/docs/rke/latest/en/upgrades/#upgrading-kubernetes)

### 1.3.4. Certificate Management

- Automatic Certificate Rotation - `rke cert rotate`

### 1.3.5. Adding and Removing Nodes

- amend the `cluster.yaml` and do `rke up`

# 2. Installing Rancher

```
$ docker run -d --restart=unless-stopped -p 80:80 -p 443:443 -v \
  /opt/rancher:/var/lib/rancher rancher/rancher:v2.4.1
```

or `rancher/rancher:stable`
- persistent data at `/var/lib/rancher` (a docker volume)
- 
- 

# 3. References
- [KMC - Hands On with K3s Support in Rancher 2.4 - 2020-06-16](https://www.youtube.com/watch?v=7tPseWagth8) (video)
- [What’s the difference between k3s vs k8s](https://www.civo.com/blog/k8s-vs-k3s)