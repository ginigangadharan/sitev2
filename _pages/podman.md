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
---

Also See **[Skopeo Cheat Sheet](skopeo)** for details.

*Note : This is s a living document and I will update whenever needed*

```
# Check version
podman --version

# Login to Registry
sudo podman login -u USER_NAME REGISTRY_URL

# Create a container 
sudo podman run -d --name TEST \
  quay.io/USER_NAME/IMAGE_NAME:VERSION

```

# Ref:
- https://cloudnweb.dev/2019/06/replacing-docker-with-podman-power-of-podman/


