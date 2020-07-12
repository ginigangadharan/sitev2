---
layout: post
title: s2i Cheat Sheet
categories: [ cloud ]
#image: "assets/images/2020/ansible-automation.png"
tags: [cloud, openshift, containers, kubernetes, source to image]
permalink: s2i
author: gini
featured: false
hidden: false
---

Also See,
- **[OpenShift-Kubernetes-Docker Cheatsheet](https://okd.iamgini.com)**
- **[Skopeo Cheat Sheet](skopeo)** for details.
- **[Podman Cheat Sheet](podman)** for details.

*Note : This is s a living document and I will update whenever needed*
```
$ s2i version

#-- Create 
$ s2i create IMAGE_NAME DESTINATION_DIR 
$ s2i create s2i-test s2i-test
$ tree -a s2i-test
s2i-test
├── Dockerfile
├── Makefile
├── README.md
├── s2i
│   └── bin
│       ├── assemble
│       ├── run
│       ├── save-artifacts
│       └── usage
└── test
    ├── run
    └── test-app
        └── index.html
4 directories, 9 files

#-- configure all templated files as needed before creating image

#-- Create the builder image

```