

- [1. Introduction to Rancher & RKE](#1-introduction-to-rancher--rke)
  - [1.1. Rancher Architecture](#11-rancher-architecture)
  - [1.2. Disocvering RKE](#12-disocvering-rke)
    - [1.2.1. Creating the Cluster Configuration File](#121-creating-the-cluster-configuration-file)
  - [1.3. Day Two Operations For RKE](#13-day-two-operations-for-rke)
    - [1.3.1. Secure the Installation Files](#131-secure-the-installation-files)
    - [1.3.2. Backups and DR](#132-backups-and-dr)
- [2. References](#2-references)


# 1. Introduction to Rancher & RKE

## 1.1. Rancher Architecture

## 1.2. Disocvering RKE

- RKE deploys Kubernetes components as docker containers
- SSH is used to orchestrate RKE across servers
- Cluster.yml contains all the information RKE needs to provision a Kubernetes cluster
- You can supply your own certificates that Rancher will serve for its UI/API
- RKE will support the Latest patch releases from the three most recent minor releases correct

### 1.2.1. Creating the Cluster Configuration File
```
$ rke config
```
Answer questions and create `cluster.yaml`

```
$ rke up --ssh-agent-auth
```

## 1.3. Day Two Operations For RKE

### 1.3.1. Secure the Installation Files

Save `kube_config_cluster.yaml` and `cluster.rkestate`

### 1.3.2. Backups and DR

- Snapshot every 6 hours and keep it for 24 hours
- or take manual snaphort `rke etcd snapshort-save`, which will save snapshot in `/opt/rke/etcd-snapshots/` directory
- minio to keep snapshots
- `rke etcd snapshot-restore --name BACKUP_FILE` to restore

# 2. References
- [KMC - Hands On with K3s Support in Rancher 2.4 - 2020-06-16](https://www.youtube.com/watch?v=7tPseWagth8) (video)
- [What’s the difference between k3s vs k8s](https://www.civo.com/blog/k8s-vs-k3s)