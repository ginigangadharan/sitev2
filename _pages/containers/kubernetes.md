---
layout: post
title: Kubernetes - Tools, Guides, References
author: gini
categories: [ kubernets ]
image: "assets/images/2021/boat-20427_1920-kubernetes-helm.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: kubernetes
featured: false
hidden: false
titleshort: Kubernetes
---

These are collection of reference documents and blog posts from different experts around.

Check **[techbeatly.com/k8s](https://techbeatly.com/k8s)** for latest promos and articles.

- [Learn Kubernetes](#learn-kubernetes)
  - [Kubernetes Certification](#kubernetes-certification)
  - [CKS-Certified-Kubernetes-Security-Specialist](#cks-certified-kubernetes-security-specialist)
  - [Learn Kubernetes from VMWare](#learn-kubernetes-from-vmware)
  - [Learn ECS and EKS](#learn-ecs-and-eks)
- [Kuberenets ToolBox](#kuberenets-toolbox)
  - [Kubernetes Cluster Management Tools](#kubernetes-cluster-management-tools)
  - [Kubernetes Development Tools](#kubernetes-development-tools)
  - [Kubernetes Monitoring Tools](#kubernetes-monitoring-tools)
  - [Kubernetes Network Policy Tools](#kubernetes-network-policy-tools)
  - [Kubernetes Service Mesh Tools](#kubernetes-service-mesh-tools)
  - [Kubernetes Security Tools](#kubernetes-security-tools)
  - [Kubernetes Tracing and Logging Tools](#kubernetes-tracing-and-logging-tools)
  - [Kubernetes Command Line Utlities](#kubernetes-command-line-utlities)
- [Career in Kubernetes](#career-in-kubernetes)
  - [Kubernetes Interview Questions](#kubernetes-interview-questions)
- [Tanzu Kubernetes Grid - TNG](#tanzu-kubernetes-grid---tng)
- [References](#references)


## Learn Kubernetes 

### Kubernetes Certification

- Read my article on Certification & Exam Tips, Learning Paths -  **[CKA, CKAD & CKS – Learning Path and Certification](https://www.techbeatly.com/2020/05/kubernetes-certification-cka-ckad-exam-tips-learning-path.html)**

**Recommended Courses** 

  - Certified Kubernetes Administrator (CKA) - [Kodekloud](https://www.techbeatly.com/kk-cka) / [Udemy](https://www.techbeatly.com/kk-cka-ud)
  - Certified Kubernetes Application Developer (CKAD) - [Kodekloud](https://www.techbeatly.com/kk-ckad) / [Udemy](https://www.techbeatly.com/kk-ckad-ud)
  - Certified Kubernetes Security Specialist (CKS) - [Kodekloud](https://www.techbeatly.com/kk-cks) / [Udemy](https://www.techbeatly.com/kw-cks)

<div class="videocontainer">
<iframe src="https://www.youtube.com/embed/tpzA3AJt2jQ" 
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen class="videoiframe"></iframe>
</div>

### CKS-Certified-Kubernetes-Security-Specialist

- [CKSS-Certified-Kubernetes-Security-Specialist](https://github.com/ijelliti/CKSS-Certified-Kubernetes-Security-Specialist)
 
### Learn Kubernetes from VMWare

- [KubeAcademy](https://kube.academy) 

### Learn ECS and EKS

- [ECS Workshop by AWS](https://ecsworkshop.com/)
- [AWS CodeDeploy now supports linear and canary deployments for Amazon ECS](https://aws.amazon.com/blogs/containers/aws-codedeploy-now-supports-linear-and-canary-deployments-for-amazon-ecs/)
- [AWS Container Security](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/security.html)
- [Serverless Workshops](https://github.com/aws-samples/aws-serverless-workshops/)
- [Building Serverless Web Applications with React and AWS Amplify](https://github.com/dabit3/aws-amplify-workshop-react)
- [WILD RYDES](https://webapp.serverlessworkshops.io/)

<div class="videocontainer">
<iframe src="https://www.youtube.com/embed/daJs-09iLpY" 
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen class="videoiframe"></iframe>
</div>

## Kuberenets ToolBox

### Kubernetes Cluster Management Tools

- **[kubespray](https://github.com/kubernetes-sigs/kubespray)** - Deploy a Production Ready Kubernetes Cluster using Ansible. 
  - **[Deploying Kubernetes with Kubespray](https://www.youtube.com/watch?v=JdgQAsEItTc)** - Video Guide
- **[kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)** - Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice "fast paths" for creating Kubernetes clusters.
- **[kops](https://github.com/kubernetes/kops)** - kOps - Kubernetes Operations
- **[k9s]([Kubernetes CLI To Manage Your Clusters In Style](https://k9scli.io/))** - K9s is a terminal based UI to interact with your Kubernetes clusters. The aim of this project is to make it easier to navigate, observe and manage your deployed applications in the wild. K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.
- ~~**[kube-up.sh](#)**~~ - deprecated.
- **[Cluster API](https://cluster-api.sigs.k8s.io/)** - Cluster API is a Kubernetes sub-project focused on providing declarative APIs and tooling to simplify provisioning, upgrading, and operating multiple Kubernetes clusters.
- **[metalk8s](https://github.com/scality/metalk8s)** - An opinionated Kubernetes distribution with a focus on long-term on-prem deployments
- **[Rancher](https://github.com/rancher/rancher)** - Rancher is an open source project that provides a container management platform built for organizations that deploy containers in production. Rancher makes it easy to run Kubernetes everywhere, meet IT requirements, and empower DevOps teams.
- **[kind](https://github.com/kubernetes-sigs/kind)** - Kubernetes IN Docker - local clusters for testing Kubernetes
- **[KubeSphere](https://github.com/kubesphere/kubesphere)** - Enterprise-grade container platform tailored for multicloud and multi-cluster management
- **[Kubernetes Instance Calculator](https://learnk8s.io/kubernetes-instance-calculator)** by [learnk8s](https://learnk8s.io) - You can use the calculator to explore the best instance types for your cluster based on your workloads.

### Kubernetes Development Tools

- **[k8slens.dev](https://k8slens.dev/)** - Kuberenetes IDE for developers
- **[portworx.com](https://install.portworx.com)** - Kubernetes spec generator
- **[containerlabs.kubedaily.com](https://containerlabs.kubedaily.com/)**
- **[50+ Useful Kubernetes Tools for 2020](https://caylent.com/50-useful-kubernetes-tools-for-2020)**
- **[copper](https://github.com/cloud66-oss/copper)** - Copper is a simple tool for validate your configuration files. This is specifically useful with Kubernetes configuration files to enforce best practices, apply policies and compliance requirements.

### Kubernetes Monitoring Tools

- **[thanos](https://github.com/thanos-io/thanos)**- Thanos is a set of components that can be composed into a highly available metric system with unlimited storage capacity, which can be added seamlessly on top of existing Prometheus deployments.
- **[prometheus](https://github.com/prometheus/prometheus)** - Prometheus, a Cloud Native Computing Foundation project, is a systems and service monitoring system. It collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts when specified conditions are observed.
- **[grafana](https://github.com/grafana/grafana)** - The open and composable observability and data visualization platform. Visualize metrics, logs, and traces from multiple sources like Prometheus, Loki, Elasticsearch, InfluxDB, Postgres and many more.
- **[Kubewatch](https://github.com/bitnami-labs/kubewatch)** - Watch k8s events and trigger Handlers
- **[cadvisor](https://github.com/google/cadvisor)** - Analyzes resource usage and performance characteristics of running containers

### Kubernetes Network Policy Tools 

- **[kubepox](https://github.com/aporeto-inc/kubepox)** - Kubernetes network Policy eXploration tool: A simple tools that allows you to query all the defined network policies, and associated affected Pods.
- **[calico](https://github.com/projectcalico/calico)** - Cloud native networking and network security
- **[kokotap](https://github.com/redhat-nfvpe/kokotap)** - Tools for kubernetes pod network tapping

### Kubernetes Service Mesh Tools

- **[traefik](https://github.com/traefik/traefik)** - The Cloud Native Application Proxy
- **[istio](https://github.com/istio/istio)** - Connect, secure, control, and observe services.
- **[kubernetes-ingress](https://github.com/nginxinc/kubernetes-ingress)** - NGINX and NGINX Plus Ingress Controllers for Kubernetes
- **[autopilot](https://docs.solo.io/autopilot/latest)** - Autopilot is an SDK and toolkit for developing and deploying service mesh operators.
- **[Network Policy Editor](https://editor.cilium.io)** - by Cilium

### Kubernetes Security Tools

- **[kubestriker](https://github.com/vchinnipilli/kubestriker)** - A Blazing fast Security Auditing tool for kubernetes!!
- **[terrascan](https://github.com/accurics/terrascan)** - Detect compliance and security violations across Infrastructure as Code to mitigate risk before provisioning cloud native infrastructure.
- **[Checkov](https://github.com/bridgecrewio/checkov)** - Prevent cloud misconfigurations during build-time for Terraform, Cloudformation, Kubernetes, Serverless framework and other infrastructure-as-code-languages with Checkov by Bridgecrew.
- **[kube-bench](https://github.com/aquasecurity/kube-bench)**- Checks whether Kubernetes is deployed according to security best practices as defined in the CIS Kubernetes Benchmark
- **[kubescape](https://github.com/armosec/kubescape)** - kubescape is the first tool for testing if Kubernetes is deployed securely as defined in Kubernetes Hardening Guidance by to NSA and CISA
- **[Falco](https://github.com/falcosecurity/falco)** - Cloud Native Runtime Security
- **[Clair](https://github.com/quay/clair)** - Vulnerability Static Analysis for Containers
- **[Open Policy Agent (OPA)](https://github.com/open-policy-agent/opa)** - An open source, general-purpose policy engine.
- **[KubeLinter](https://github.com/stackrox/kube-linter)** - A static analysis tool that checks Kubernetes YAML files and Helm charts to ensure the applications represented in them adhere to best practices.
- **[Kube-hunter](https://github.com/aquasecurity/kube-hunter)** - Hunt for security weaknesses in Kubernetes clusters.
- **[anchore-engine](https://github.com/anchore/anchore-engine)** - A service that analyzes docker images and applies user-defined acceptance policies to allow automated container image validation and certification
- **[grafeas](https://github.com/grafeas/grafeas)** - open-source artifact metadata API that provides a uniform way to audit and govern your software supply chain.
- **[OpenSCAP](https://github.com/OpenSCAP)** - multiple tools.
- **[Kubeaudit](https://github.com/Shopify/kubeaudit)** -  audit your Kubernetes clusters against common security controls
- [Kubernetes Hardening Guidance](https://www.nsa.gov/News-Features/Feature-Stories/Article-View/Article/2716980/nsa-cisa-release-kubernetes-hardening-guidance/)

### Kubernetes Tracing and Logging Tools

- **[Loki](https://github.com/grafana/loki)** - like Prometheus, but for logs.
- **[elastic](https://www.elastic.co/what-is/kubernetes-monitoring)** - Bring Kubernetes logs, metrics, and traces together
- **[Kiali](https://github.com/kiali/kiali)**- Kiali provides answers to the questions: What microservices are part of my Istio service mesh and how are they connected?
- **[jaeger](https://github.com/jaegertracing/jaeger)** - Distributed Tracing System

### Kubernetes Command Line Utlities

- **[Kubectl Command Cheatsheet](https://www.bluematador.com/learn/kubectl-cheatsheet)**
- **[kuttle](https://github.com/kayrus/kuttle)** - kubectl wrapper for sshuttle without SSH
  
## Career in Kubernetes

### Kubernetes Interview Questions

- [30 Best Kubernetes Interview Questions and Answers](https://www.whizlabs.com/blog/top-kubernetes-interview-questions/)

## Tanzu Kubernetes Grid - TNG

- [VMWare Tanzu](vmware-tanzu)

## References

- [Kubernetes Configuration & Best Practices](https://bcouetil.gitlab.io/academy/BP-kubernetes.html)
- [Kubernetes NodePort vs LoadBalancer vs Ingress? When should I use what?](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0)
- [Goodbye Docker Desktop, Hello Minikube!](https://itnext.io/goodbye-docker-desktop-hello-minikube-3649f2a1c469)

