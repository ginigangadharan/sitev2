---
layout: post
title: OpenShift - Tools, Learning Guides and References
author: gini
categories: [ cloud ]
image: "assets/images/2020/how-to-create-increase-or-decrease-project-quota-in-openshift.jpg"
tags: [cloud, automation, containers, kubernetes]
#show-avatar: false
#bigimg: /img/Kubernetes_1920x1080.jpg
permalink: openshift
featured: false
hidden: false
showindex: true
---

Learn OpenShift with us via **[OpenShift BootCamp](http://techbeatly.com/openshift-bootcamp) ** Free course.

Also check,

- **[OpenShift-Kubernetes-Docker Cheatsheet](https://okd.iamgini.com)**
- **[Skopeo Cheat Sheet](skopeo)** for details.
- **[Podman Cheat Sheet](s2i)** for details.

- [OpenShift - Questions and Answers](#openshift---questions-and-answers)
  - [What are the features of OpenShift ?](#what-are-the-features-of-openshift-)
  - [How to set number of pods per node in OpenShift ?](#how-to-set-number-of-pods-per-node-in-openshift-)
  - [What is a Distributed System ?](#what-is-a-distributed-system-)
  - [TODO/ What is ODF ?](#todo-what-is-odf-)
  - [TODO/ What is ServiceMesh ?](#todo-what-is-servicemesh-)
  - [What is Kiali ?](#what-is-kiali-)
  - [What is Jeager ?](#what-is-jeager-)
  - [TODO/ What is Kubernetes Admission Controller ?](#todo-what-is-kubernetes-admission-controller-)
  - [How Container Security Works ?](#how-container-security-works-)
  - [What is `--pod-eviction-timeout` in Kubernetes ?](#what-is---pod-eviction-timeout-in-kubernetes-)
  - [TODO/ What is Red Hat Nooba ?](#todo-what-is-red-hat-nooba-)
  - [What is Air-Gapped (disconnected) OpenShift Clusters](#what-is-air-gapped-disconnected-openshift-clusters)
  - [What is new in Red Hat OpenShift 4 ?](#what-is-new-in-red-hat-openshift-4-)
  - [What is Clair ?](#what-is-clair-)
  - [What is Quay ?](#what-is-quay-)
  - [TODO/ What is CoreOS and Why using CoreOS for OpenShift ?](#todo-what-is-coreos-and-why-using-coreos-for-openshift-)
  - [What is CRI-O ?](#what-is-cri-o-)
  - [TODO/ What is `etcd` ?](#todo-what-is-etcd-)
  - [OpenShift Hardering - What is Compliance Operator ?](#openshift-hardering---what-is-compliance-operator-)
  - [TODO/ What is Kubernetes Operator (or OpenShift Operator) ?](#todo-what-is-kubernetes-operator-or-openshift-operator-)
  - [What is Operator Framework ?](#what-is-operator-framework-)
  - [What Red Hat Advanced Cluster Security for Kubernetes (RHACS) ?](#what-red-hat-advanced-cluster-security-for-kubernetes-rhacs-)
  - [What are the components of OpenShift Cluster Monitoring, Logging and Telemetry ?](#what-are-the-components-of-openshift-cluster-monitoring-logging-and-telemetry-)
  - [What is `cadvisor` in kubernetes ?](#what-is-cadvisor-in-kubernetes-)
  - [TODO/ What is Helm ?](#todo-what-is-helm-)
  - [What are the points to note while architecting OpenShift Clusters ?](#what-are-the-points-to-note-while-architecting-openshift-clusters-)
  - [How to Manage Roles and Permissons in OpenShift ?](#how-to-manage-roles-and-permissons-in-openshift-)
  - [What is SRE and DevOps ?](#what-is-sre-and-devops-)
  - [TODO/ How to Enabled OpenShift Node AutoScaling ?](#todo-how-to-enabled-openshift-node-autoscaling-)
  - [What is KeyStone ?](#what-is-keystone-)
  - [Removing the kubeadmin user](#removing-the-kubeadmin-user)
  - [What is cgroup ? (CONTROL GROUPS)](#what-is-cgroup--control-groups)
  - [What is Multus Container Network Interface (CNI) ?](#what-is-multus-container-network-interface-cni-)
  - [What is blue green deployment?](#what-is-blue-green-deployment)
  - [How to Size an OpenShift Cluster ?](#how-to-size-an-openshift-cluster-)
- [OpenShift Installation](#openshift-installation)
- [OpenShift Migration and Upgrades](#openshift-migration-and-upgrades)
- [OpenShift Tools, Integrations & Terminologies](#openshift-tools-integrations--terminologies)
  - [OpenShift `odo`](#openshift-odo)
  - [kube-burner](#kube-burner)
- [CI/CD on OpenShift](#cicd-on-openshift)
- [OpenShift Articles](#openshift-articles)
- [Azure Red Hat OpenShift (ARO)](#azure-red-hat-openshift-aro)
- [Learn OpenShift](#learn-openshift)

## OpenShift - Questions and Answers

*Note: These are questions I have received via chat groups and communities. This is a living document and I will update the page whenever there is a new question or better answer or references.*

### What are the features of OpenShift ?

- High Availabilty
- Lightweight Operating System
- Load Balancing
- Automated Scaling
- Logging and Monitoring (based on prometheus, ElasticSearch)
- Service Discovery
- Storage
- Application Management
- Cluster Extensibility

### How to set number of pods per node in OpenShift ?

- [How to set number of pods per node in OpenShift ? - Quick Guide](how-to-set-number-of-pods-per-node-openshift)
- [OpenShift Scale: Running 500 Pods Per Node](https://www.openshift.com/blog/500_pods_per_node)
- [Managing the maximum number of pods per node](https://docs.openshift.com/container-platform/4.6/nodes/nodes/nodes-nodes-managing-max-pods.html)

### What is a Distributed System ?

A distributed system in its most simplest definition is a group of computers working together as to appear as a single computer to the end-user. These machines have a shared state, operate concurrently and can fail independently without affecting the whole system’s uptime. 

Read more : [A Thorough Introduction to Distributed Systems](https://www.freecodecamp.org/news/a-thorough-introduction-to-distributed-systems-3b91562c9b3c/)

- Fault Tolerance / High Availability
- Low Latency 

### TODO/ What is ODF ?

### TODO/ What is ServiceMesh ?

- [OpenShift and Service Mesh](openshift-servicemesh)
- [Installing Red Hat OpenShift Service Mesh](https://docs.openshift.com/container-platform/4.7/service_mesh/v1x/installing-ossm.html)
  - Install Elastic Operator
  - Install Jeager Operator
  - Install Kiali Operator
  - Install Red Hat OpenShift Service Mesh (OSSM) Operator

### What is Kiali ?

Kiali is a management console for an Istio-based service mesh. It provides dashboards, observability, and lets you operate your mesh with robust configuration and validation capabilities. It shows the structure of your service mesh by inferring traffic topology and displays the health of your mesh.

- [Website](https://kiali.io/) / [GitHub](https://github.com/jaegertracing/jaeger)

### What is Jeager ?

Distributed Tracing System - Monitor and troubleshoot transactions in complex distributed systems

- [website][https://www.jaegertracing.io]

### TODO/ What is Kubernetes Admission Controller ?
- `MutatingAdmissionWebhook` and `ValidatingAdmissionWebhook`

- [Using Admission ControllersUsing Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)

### How Container Security Works ?
-  Secure the container host
-  Secure the networking environment -  intrusion prevention system (IPS) and web filtering for traffic moving from north to south, and to and from the internet, in order to stop attacks and filter malicious content.
-  Secure your management stack - container registry, Kubernetes installation, network policies.
-  Build on a secure foundation - patching, updates
-  Secure your build pipeline
-  Secure your application

- [Container Security in Six Steps](https://www.trendmicro.com/vinfo/sg/security/news/security-technology/container-security-in-six-steps)

### What is `--pod-eviction-timeout` in Kubernetes ?

**The grace period for deleting pods on failed nodes.** - The default eviction timeout duration is five minutes. In some cases when the node is unreachable, the API server is unable to communicate with the kubelet on the node. The decision to delete the pods cannot be communicated to the kubelet until communication with the API server is re-established.

### TODO/ What is Red Hat Nooba ?

### What is Air-Gapped (disconnected) OpenShift Clusters

Air-gapped environments are those that are physically isolated from other networks, but most importantly isolated from the Internet. No proxies, no jump hosts - nothing. 

- [Is your Operator Air-Gap Friendly?](https://www.openshift.com/blog/is-your-operator-air-gap-friendly)
- OpenShift 4 in an Air Gap (disconnected) environment- [Part1](https://two-oes.medium.com/openshift-4-in-an-air-gap-disconnected-environment-part-1-prerequisites-63f065e8a729), [Part2](https://two-oes.medium.com/openshift-4-in-an-air-gap-disconnected-environment-part-2-installation-1dd8bf085fdd)

### What is new in Red Hat OpenShift 4 ?

- Immutable Red Hat Enterprise Linux CoreOS
- Fullstack Automation
- OpenShift service mesh
- Operator framework
- Knative framework
- Managing Multiple Clusters Across Multiple Clouds

Read : [Introducing Red Hat OpenShift 4: Kubernetes for the Enterprise](https://www.openshift.com/blog/introducing-red-hat-openshift-4#:~:)

### What is Clair ?

Clair is an open source project for the static analysis of vulnerabilities in application containers (currently including OCI and docker).

- [Clair GitHub](https://github.com/quay/clair)

### What is Quay ?

Quay is a container image registry that enables you to build, organize, distribute, and deploy containers. This is the Community Distribution of Quay that powers Red Hat Quay and Quay.io

- [ProjectQuay][https://www.projectquay.io/]
  
### TODO/ What is CoreOS and Why using CoreOS for OpenShift ?

CoreOS was founded in 2013 with the mission to improve the security and reliability of the internet. *On May 26, 2020, CoreOS Container Linux reached its end of life and will no longer receive updates.*
-  It’s the successor to both Fedora Atomic Host and CoreOS Container Linux
-  Based on RHEL
-  Controlled immutability
-  CRI-O container runtime
-  Set of container tools: `podman`, `skopeo`, `buildah`, `crictl`

**References**
- [What was CoreOS](https://www.openshift.com/learn/topics/coreos)
- [Getting Started with CoreOS](https://www.digitalocean.com/community/tutorial_series/getting-started-with-coreos-2)
- [Producing an Ignition Config](https://docs.fedoraproject.org/en-US/fedora-coreos/producing-ign/)

### What is CRI-O ?

LIGHTWEIGHT CONTAINER RUNTIME FOR KUBERNETES

CRI-O is an implementation of the Kubernetes CRI (Container Runtime Interface) to enable using OCI (Open Container Initiative) compatible runtimes. It is a lightweight alternative to using Docker as the runtime for kubernetes. It allows Kubernetes to use any OCI-compliant runtime as the container runtime for running pods. 

- `cri` - container runtime interface came in picture as new container engines supports included in kubernetes.
- `cni` - container network interface is for networking support
- `csi` - container storage interface is to support storage drivers and solutions

- [Website](https://cri-o.io/)
- 
### TODO/ What is `etcd` ?

`etcd` is a distributed key-value store.

### OpenShift Hardering - What is Compliance Operator ?

The Compliance Operator lets OpenShift Container Platform administrators describe the desired compliance state of a cluster and provides them with an overview of gaps and ways to remediate them. 

- [Understanding the Compliance Operator](https://docs.openshift.com/container-platform/4.7/security/compliance_operator/compliance-operator-understanding.html)
- How does Compliance Operator work for OpenShift? [Part 1](https://www.openshift.com/blog/how-does-compliance-operator-work-for-openshift-part-1), [Part 2](https://www.openshift.com/blog/how-does-compliance-operator-work-for-openshift-part-2)
- [Compliance Operartor - GitHub](https://github.com/openshift/compliance-operator)
- [Understanding the Compliance Operator](https://docs.openshift.com/container-platform/4.6/security/compliance_operator/compliance-operator-understanding.html)
- Also read - [kube-bench](https://github.com/aquasecurity/kube-bench) is a Go application that checks whether Kubernetes is deployed securely by running the checks documented in the [CIS Kubernetes Benchmark](https://www.cisecurity.org/benchmark/kubernetes/).

### TODO/ What is Kubernetes Operator (or OpenShift Operator) ?

Use the Kubernetes API to create, configure, and automatically manage applications.
Operator have 2 components;
  1. CRD - Custom Resource Definition
  2. Controller - which controls the application deployment.

- Use Helm/Ansible/Go
- `OLM` stands for Operator Lifecyle Management
- [What is an Operator - OpenShift.com](https://www.openshift.com/learn/topics/operators)
- [Troubleshooting OpenShift Operator issues](https://docs.openshift.com/container-platform/4.5/support/troubleshooting/troubleshooting-operator-issues.html)
- [Building Kubernetes Operators with the Operator Framework and Ansible](https://www.youtube.com/watch?v=5XZZxhwb_xs) 
### What is Operator Framework ?

**5 Levels of Operator Framework**

1. Basic Install
2. Seamless Upgrade
3. Full Lifecycle
4. Deep Insight
5. Auto Pilot

**Operator Components**

- Operator SDK - Provides tooling to build and package operators
- Operator Lifecyle Management (OLM) - Governs operator scope and lifecycle
- Metering - historical information to be gathered and reported on
- OperatorHub - marketplace for community operators


### What Red Hat Advanced Cluster Security for Kubernetes (RHACS) ?

Red Hat® Advanced Cluster Security for Kubernetes, powered by StackRox technology, protects your vital applications across build, deploy, and runtime.

- Control the trusted sources of content
- Defend applications from attachs and vulnerabilities
- Extend secure services through standard interface and API's

Also Read;
- [Red Hat Advanced Cluster Security for Kubernetes](https://www.redhat.com/en/resources/advanced-cluster-security-for-kubernetes-datasheet)
- [Red Hat OpenShift container security](https://www.openshift.com/learn/topics/security/)
- [OpenShift Security Best Practices for Kubernetes Cluster Design](https://www.openshift.com/blog/openshift-security-best-practices-for-kubernetes-cluster-design)

### What are the components of OpenShift Cluster Monitoring, Logging and Telemetry ?

- **collection** - This is the component that collects logs from the cluster, formats them, and forwards them to the log store. The current implementation is **Fluentd**.
- **log store** - This is where the logs are stored. The default implementation is **Elasticsearch**. You can use the default Elasticsearch log store or forward logs to external log stores. The default log store is optimized and tested for short-term storage.
- **visualization** - This is the UI component you can use to view logs, graphs, charts, and so forth. The current implementation is **Kibana**.

- [Installing OpenShift Logging](https://docs.openshift.com/container-platform/4.7/logging/cluster-logging-deploying.html)

**Telemetry** 

Sends a carefully chosen subset of the cluster monitoring metrics to Red Hat. 

- [openshift/telemeter](https://github.com/openshift/telemeter)

### What is `cadvisor` in kubernetes ?

cAdvisor is an open-source agent integrated into the kubelet binary that monitors resource usage and analyzes the performance of containers. It collects statistics about the CPU, memory, file, and network usage for all containers running on a given node (it does not operate at the pod level)

- [Tools for Monitoring Resources](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-usage-monitoring/)
- [Native Kubernetes Monitoring, Part 1: Monitoring and Metrics for Users](https://rancher.com/blog/2019/native-kubernetes-monitoring-tools-part-1)
- [MONITORING DOCKER CONTAINER METRICS USING CADVISOR](https://prometheus.io/docs/guides/cadvisor/)

### TODO/ What is Helm ?

### What are the points to note while architecting OpenShift Clusters ? 

- [Enterprise OpenShift Deployment: What do you need to know?](https://www.openshift.com/blog/enterprise-openshift-deployment)

### How to Manage Roles and Permissons in OpenShift ?

- [Using RBAC to define and apply permissions](https://docs.openshift.com/container-platform/4.7/authentication/using-rbac.html)

### What is SRE and DevOps ?

- [What is SRE (site reliability engineering)? DevOps vs. SRE](https://www.redhat.com/en/topics/devops/what-is-sre)

### TODO/ How to Enabled OpenShift Node AutoScaling ?

- [Applying autoscaling to an OpenShift Container Platform cluster](https://docs.openshift.com/container-platform/4.7/machine_management/applying-autoscaling.html)


### What is KeyStone ?

Keystone is an OpenStack project that provides identity, token, catalog, and policy services. You can configure the integration with Keystone so that the new OpenShift Container Platform users are based on either the Keystone user names or unique Keystone IDs. 

- [What is keystone federation?](https://docs.openstack.org/keystone/latest/admin/federation/introduction.html)

### Removing the kubeadmin user

After you define an identity provider and create a new `cluster-admin` user, you can remove the `kubeadmin` to improve cluster security.

***Warning:** If you follow this procedure before another user is a cluster-admin, then OpenShift Container Platform must be reinstalled. It is not possible to undo this command.*

**Prerequisites**

- You must have configured at least one identity provider.
- You must have added the cluster-admin role to a user.
- You must be logged in as an administrator.

```bash
-- Remove the kubeadmin secrets:
$ oc delete secrets kubeadmin -n kube-system
```

### What is cgroup ? (CONTROL GROUPS)

`cgroups` is a Linux kernel feature that limits, accounts for, and isolates the resource usage of a collection of processes.

- [Understanding resource limits in kubernetes: memory](https://medium.com/@betz.mark/understanding-resource-limits-in-kubernetes-memory-6b41e9a955f9)
- [NTRODUCTION TO CONTROL GROUPS (CGROUPS)](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/ch01)
- [Linux Control Groups and Process Isolation](https://www.linuxjournal.com/content/everything-you-need-know-about-linux-containers-part-i-linux-control-groups-and-process)

### What is Multus Container Network Interface (CNI) ?

Multus CNI enables attaching multiple network interfaces to pods in Kubernetes.

- [k8snetworkplumbingwg/multus-cni](https://github.com/k8snetworkplumbingwg/multus-cni)
- [Using the Multus CNI in OpenShift](https://www.openshift.com/blog/using-the-multus-cni-in-openshift)

### What is blue green deployment?

Blue green deployment is an application release model that gradually transfers user traffic from a previous version of an app or microservice to a nearly identical new release—both of which are running in production. 

- [What is blue green deployment?](https://www.redhat.com/en/topics/devops/what-is-blue-green-deployment)

### How to Size an OpenShift Cluster ?

- [OpenShift sizing and subscription guide for enterprise Kubernetes](https://www.redhat.com/en/resources/OpenShift-subscription-sizing-guide-detail)
- [Everything about Infra nodes](https://www.youtube.com/watch?v=9VNjDh1vPXI) - Video


## OpenShift Installation

- [OpenShift Installation Methods - Examples](openshift-installation)
- [CodeReady Containers - CRC](openshift-crc)
- [OpenShift 3.x Installation on GCP](openshift-3x-installation-gcp)
- [OpenShift Installation on GCP with Nested Vrtualization](openshift-gcp-nested-virt)

## OpenShift Migration and Upgrades

- [Updating a cluster within a minor version using the CLI](https://docs.openshift.com/container-platform/4.7/updating/updating-cluster-cli.html)

```shell
$ oc get clusterversion

# view available upgrades
$ oc adm upgrade

$ oc adm upgrade --to-latest=true
# or
$ oc adm upgrade --to=<version>

## Changing the update server by using the CLI
$ oc patch clusterversion/version \
  --patch '{"spec":{"upstream":"<update-server-url>"}}' \
  --type=merge
```

- [Migrating from OpenShift Container Platform 3 to 4](https://docs.openshift.com/container-platform/4.8/migrating_from_ocp_3_to_4/about-migrating-from-3-to-4.html)
- [Migration Toolkit for Containers (MTC)](https://docs.openshift.com/container-platform/4.8/migration_toolkit_for_containers/about-mtc.html)
- Read [Differences between OpenShift Container Platform 3 and 4](https://docs.openshift.com/container-platform/4.8/migrating_from_ocp_3_to_4/planning-migration-3-4.html)
- [Understanding the OpenShift Update Service](https://docs.openshift.com/container-platform/4.7/updating/understanding-the-update-service.html) - OpenShift Update Service (OSUS) 

## OpenShift Tools, Integrations & Terminologies

### OpenShift `odo`

[`odo`](https://odo.dev/) is a CLI tool for creating applications on OpenShift Container Platform and Kubernetes.

- [Understanding odo](https://docs.openshift.com/container-platform/4.6/cli_reference/developer_cli_odo/understanding-odo.html)
- [odo: Developers CLI for OpenShift and Kubernetes](https://developers.redhat.com/products/odo/overview)
- [Installing odo](https://github.com/openshift/odo)
- [odo CLI Reference](https://docs.openshift.com/container-platform/4.6/cli_reference/developer_cli_odo/odo-cli-reference.html)

### kube-burner
Kube-burner is a tool designed to stress different OpenShift components basically by coordinating the creation and deletion of k8s resources.

- [Introducing kube-burner, A tool to Burn Down Kubernetes and OpenShift](https://www.openshift.com/blog/introducing-kube-burner-a-tool-to-burn-down-kubernetes-and-openshift)

## CI/CD on OpenShift

- [Running Jenkins Pipelines on OpenShift](openshift-jenkins)

## OpenShift Articles
-  Read OpenShift articles in [techbeatly.com](https://www.techbeatly.com/category/cloud/openshift/)
-  [OpenShift Community of Practice - uncontained.io](http://uncontained.io/)

## Azure Red Hat OpenShift (ARO)

- [Azure Red Hat OpenShift 4 Documentation](https://docs.openshift.com/aro/4/welcome/index.html)
- [Tutorial: Create an Azure Red Hat OpenShift 4 cluster](https://docs.microsoft.com/en-us/azure/openshift/tutorial-create-cluster)
- 
## Learn OpenShift

- **[Ansible Operator in OpenShift](https://learn.openshift.com/ansibleop/ansible-operator-overview/)**