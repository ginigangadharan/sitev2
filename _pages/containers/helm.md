---
layout: postpage
title: Helm Chart
categories: [ Kubernetes, DevOps ]
#image: "assets/images/2020/ansible-automation.png"
tags: [ aap, cloud, automation, containers, kubernetes, ansible automation platform, red hat ansible]
permalink: helm
featured: false
hidden: false
titleshort: helm
---

[Using Helm](https://helm.sh/docs/intro/using_helm)
[Helm Commands](https://helm.sh/docs/helm/)

- [Installing Helm](#installing-helm)
- [Helm Repositoty aka Artifact Hub](#helm-repositoty-aka-artifact-hub)
- [Managing Helm Chart Repositories](#managing-helm-chart-repositories)
- [Creating Helm chart](#creating-helm-chart)
- [Deploying applications using helm](#deploying-applications-using-helm)
- [Remove deployment](#remove-deployment)
- [Download helm charts](#download-helm-charts)
- [Package and Distribute helm charts](#package-and-distribute-helm-charts)
- [Using variables](#using-variables)
- [Managing releases](#managing-releases)
- [References](#references)

## Installing Helm

[Installation doc](https://helm.sh/docs/intro/install/)


Use scripts or package managers.

```shell
$ curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

## or
$ brew install helm
```

Check version

```shell
$ helm version
version.BuildInfo{Version:"v3.8.0", GitCommit:"d14138609b01886f544b2025f5000351c9eb092e", GitTreeState:"clean", GoVersion:"go1.17.6"}

$ helm version --short
v3.8.0+gd141386
```

## Helm Repositoty aka Artifact Hub

[Artifact Hub](https://artifacthub.io/) is the place where you can find the Kubernetes package bundles.

## Managing Helm Chart Repositories

```shell
helm repo add stable REPO_URL
helm repo add stable https://charts.helm.sh/stable

helm repo list                    # list repos

helm search repo [keyword] [flags]
helm search repo stable
helm search repo stable/tomcat    # search for repos
helm search hub                   # search in the artifact hub

helm repo update                  # update local helm charts from repo

helm env                          # show environment
helm list                         # display deployed apps
```

Updating helm local chart list

```shell
$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈
```

## Creating Helm chart

```shell
$ helm create my-chart
```

The chart skeleton will be created in a directory as shown below.

```shell
$ tree myapp-chart 
myapp-chart
├── Chart.yaml
├── charts
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   ├── serviceaccount.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

3 directories, 10 files
```

- `Chart.yaml` - metadata about the chart
- `charts` - directory containing other charts
- `templates` - k8s manifest template files
- `templates/helpers.tpl` - Go template helpes
- `templates/hpa.yaml` - Kubernetes horizontal pod autoscaler (HPA) template
- `templates/values.yaml` - default values for the chart

## Deploying applications using helm

- [Helm Search Repo](https://helm.sh/docs/helm/helm_search_repo/)

*Note: Make sure you have access to Kubernetes or OpenShift cluster from your current environment.*

Search for repos and find the appropriate one.

```shell
$ helm search repo nginx

## search for pre-releases
$ helm search repo nginx --devel  

## search for specific versions
$ helm search repo nginx-ingress --version ^1.0.0
```

Select and add the repo (in this case bitnami)

```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
```

Install application (eg: `bitnami/nginx`)

```
$ helm install nginx-web-server bitnami/nginx
NAME: nginx-web-server
LAST DEPLOYED: Sat Jan 29 12:14:33 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 9.7.5
APP VERSION: 1.21.6

** Please be patient while the chart is being deployed **

NGINX can be accessed through the following DNS name from within your cluster:

    nginx-web-server.default.svc.cluster.local (port 80)

To access NGINX from outside the cluster, follow the steps below:

1. Get the NGINX URL by running these commands:

  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        Watch the status with: 'kubectl get svc --namespace default -w nginx-web-server'

    export SERVICE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].port}" services nginx-web-server)
    export SERVICE_IP=$(kubectl get svc --namespace default nginx-web-server -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
```

You can also deploy application using local helm charts by `helm install APP_NAME HELM_CHART_DIRECTORY`

```shell
helm install nodejs-demoapp helm-chart-efk-stack
```

Verify the deployment

```shell
$ kubectl get all
NAME                                  READY   STATUS    RESTARTS   AGE
pod/nginx-web-server-7d9489c4-xzbz9   1/1     Running   0          5m50s

NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes         ClusterIP      10.96.0.1       <none>        443/TCP        129m
service/nginx-web-server   LoadBalancer   10.111.20.142   <pending>     80:32565/TCP   5m50s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-web-server   1/1     1            1           5m50s

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-web-server-7d9489c4   1         1         1       5m50s
```

## Remove deployment

```shell
## remove the release from the cluster
$ helm uninstall nginx-web-server 
## delete deployment and release history
$ helm delete nginx-web-server 
```

## Download helm charts

```shell
helm pull bitnami/nginx
```
## Package and Distribute helm charts

```shell
helm package ./myapp-chart 
Successfully packaged chart and saved it to: /Users/gini/workarea/cp/myapp-chart-0.1.0.tgz
```

## Using variables

```shell
helm install my-app-1 my-app --values app-1.values.yaml
helm install my-app-2 my-app --values app-2.values.yaml
```


## Managing releases

```shell
helm install [NAME] [CHART]
helm upgrade [RELEASE][CHART]
helm rollback <RELEASE> [REVISION] [flags]
helm history [NAME]
```

## References

- [Helm – Installation and Configuration](https://kodekloud.com/helm-installation-and-configuration/)
- [Getting started with Helm](https://mnpaa1991.medium.com/getting-started-with-helm-7f4826c58426)
- [https://leeyoongti.medium.com/helm-in-kubernetes-part-1-introduction-81c810c8f486](https://leeyoongti.medium.com/helm-in-kubernetes-part-1-introduction-81c810c8f486)
- [DevOps Tools: Introducing Helmfile](https://joachim8675309.medium.com/devops-tools-introducing-helmfile-f7c0197f3aea)