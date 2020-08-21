---
layout: post
title: Podman Cheat Sheet
categories: [ cloud ]
#image: "assets/images/2020/ansible-automation.png"
tags: [cloud, openshift, containers, kubernetes]
permalink: podman
author: gini
featured: false
hidden: false
titleshort: Podman
---

Also see,
- **[OpenShift-Kubernetes-Docker Cheatsheet](https://okd.iamgini.com)**
- **[Skopeo Cheat Sheet](skopeo)** for details.
- **[Podman Cheat Sheet](s2i)** for details.

*Note : This is s a living document and I will update whenever needed*

```
$ podman --version                    # Check version

$ sudo podman login -u USER_NAME REGISTRY_URL
                                    # Login to Registry
$ sudo podman login -u USER_NAME \
  -p ${TOKEN} \
  REGISTRY_URL  
                                    # Login with token or password
                                    # eg: in OpenShift, token can retrive as
                                    # $ TOKEN=$(oc whoami -t)

$ podman logout quay.io             # Remove login credentials for registry.redhat.io
$ podman logout --all               # Remove login credentials for all registries

$ podman search REGISTRY_URL/IMAGE_NAME
                                    # search for an image in registry

$ sudo podman run --name test -u 1234 \
  -p 8080:8080 -d s2i-sample-app

$ sudo podman run -d --name TEST \
  quay.io/USER_NAME/IMAGE_NAME:VERSION
                                    # Create a container 

$ sudo podman ps                    # List running containers
$ sudo podman stop CONTAINER_NAME   # STOP running containers
$ sudo podman rm CONTAINER_NAME     # remove running containers

$ sudo podman logs CONTAINER_NAME                    
                                    # check logs of running container

$ sudo podman build -t NAME .       # build container image from Dockerfile and spec
$ sudo podman images                # see available images

```

# Reference
- **[Replacing Docker with Podman - Power of Podman](https://cloudnweb.dev/2019/06/replacing-docker-with-podman-power-of-podman/)**


