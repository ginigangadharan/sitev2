---
layout: post
title: Running Jenkins Pipelines on OpenShift
author: gini
categories: [ jenkins, openshift ]
image: "assets/images/2020/how-to-create-increase-or-decrease-project-quota-in-openshift.jpg"
tags: [cloud, automation, containers, openshift]
#show-avatar: false
#bigimg: /img/Kubernetes_1920x1080.jpg
permalink: openshift-jenkins
featured: false
hidden: false
showindex: true
titleshort: openshift-jenkins
---

- `JenkinsPipelineStrategy` - OpenShift provides this build type.
- Entire build lifecycle can be monitored and managed from OpenShift.
- Define pipelines and workflows in `Jenkinsfile` or a git repository

- [1. Deploy Jenkins in OpenShift](#1-deploy-jenkins-in-openshift)
- [Configure App Project](#configure-app-project)
- [create project for app](#create-project-for-app)
- [de](#de)


## 1. Deploy Jenkins in OpenShift
```bash
## Check available jenkins template
$ oc get templates -n openshift | grep jenkins

## Create a new project 
$ oc new-project jenkins-project

## Deploy jenkins and wait for the deployment up and running
$ oc new-app  --as-deployment-config \
> jenkins-ephemeral -p MEMORY_LIMIT=2048Mi

$ oc get pod
## find the jenkins app url
$ oc get route

```

<image>

Login with OpenShift credentials and Authorize Access.

## Configure App Project

```bash
## create project for app
$ oc new-project cicd-app-demo

## de
