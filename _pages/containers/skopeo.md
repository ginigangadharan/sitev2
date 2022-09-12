---
layout: post
title: Skopeo Cheat Sheet
categories: [ cloud ]
#image: "assets/images/2020/ansible-automation.png"
tags: [cloud, openshift, containers, kubernetes]
permalink: skopeo
author: gini
featured: false
hidden: false
titleshort: skopeo
---

Also see,
- **[OpenShift-Kubernetes-Docker Cheatsheet](https://okd.iamgini.com)**
- **[Skopeo Cheat Sheet](skopeo)** for details.
- **[Podman Cheat Sheet](podman)** for details.
  
*Note : This is s a living document and I will update whenever needed*

- [Skopeo Handy Commands](#skopeo-handy-commands)
  - [Copy local oci image to destination](#copy-local-oci-image-to-destination)
  - [copy using credential to destination](#copy-using-credential-to-destination)
  - [inspect the image](#inspect-the-image)
  - [delete an image](#delete-an-image)
- [References](#references)

# Skopeo Handy Commands 

## Copy local oci image to destination
```
$ skopeo copy \
  oci:/home/USER/SOURCE_PATH \
  docker://quay.io/USER/IMAGE_NAME:VER
```
## copy using credential to destination

```
$ skopeo copy \
  --dest-creds=USER_NAME:TOKEN \
  oci:/home/USER/SOURCE_PATH \
  docker://REGISTRY_URL/USER/IMAGE_NAME:VER

$ sudo skopeo copy \
  containers-storage:localhost/IMAGE_NAME \
  docker://REGISTRY_URL/USER/IMAGE_NAME:VER
```

## inspect the image                               

```
$ skopeo inspect REGISTRY_URL/IMAGE_NAME
```

## delete an image

```
$ sudo skopeo delete \
  docker://REGISTRY_URL/USER/IMAGE_NAME:VER
```

# References

- **[Skopeo Repo](https://github.com/containers/skopeo)**
- **[Say “Hello” to Buildah, Podman, and Skopeo](https://servicesblog.redhat.com/2019/10/09/say-hello-to-buildah-podman-and-skopeo/)** - Red Hat Blog
- **[Promoting container images between registries with skopeo](https://www.openshift.com/blog/promoting-container-images-between-registries-with-skopeo)**
- **[Skopeo Help](https://www.systutorials.com/docs/linux/man/1-skopeo/)**
- **[Skopeo 1.0 released](https://www.redhat.com/en/blog/skopeo-10-released)**
