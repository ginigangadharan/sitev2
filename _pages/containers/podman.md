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
- **[OpenShift Learning Center](openshift)**
- **[OpenShift-Kubernetes-Docker Cheatsheet](https://okd.iamgini.com)**
- **[Skopeo Cheat Sheet](skopeo)** for details.
- **[Podman Cheat Sheet](s2i)** for details.

*Note : This is s a living document and I will update whenever needed*

- [Using Podman inside Container](#using-podman-inside-container)
- [Podman Machine on MacOS](#podman-machine-on-macos)
- [Reference](#reference)
- [Troubleshooting](#troubleshooting)
  - [Unable to connect to podman - podman machine](#unable-to-connect-to-podman---podman-machine)

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

$ podman run --privileged quay.io/podman/stable podman run ubi8 echo hello
                                    # The easiest way to run Podman inside of a container is to use the --privileged flag.

$ sudo podman ps                    # List running containers
$ sudo podman stop CONTAINER_NAME   # STOP running containers
$ sudo podman rm CONTAINER_NAME     # remove running containers

# sudo podman rmi IMAGE_NAME        # delete container image
$ sudo podman logs CONTAINER_NAME                    
                                    # check logs of running container

$ sudo podman build -t NAME .       # build container image from Dockerfile and spec
$ sudo podman images                # see available images

```

## Using Podman inside Container


- [How to use Podman inside of a container](https://www.redhat.com/sysadmin/podman-inside-container)

**Side notes**

```shell
# add DNS, enable and start systemd-resolved
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
```

```shell
## Rootful Podman in rootful Podman with --privileged
podman run --privileged quay.io/podman/stable podman run ubi8 echo hello

## added volume
podman run --privileged -v ./mycontainers:/var/lib/containers quay.io/podman/stable podman run ubi8 echo hello

## Rootless Podman in rootful Podman with --privileged
podman run --user podman --privileged quay.io/podman/stable podman run ubi8 echo hello

## Rootful Podman in rootful Podman without --privileged
podman run --cap-add=sys_admin,mknod --device=/dev/fuse --security-opt label=disable quay.io/podman/stable podman run ubi8-minimal echo hello
```

Sample

```shell
## Run podman inside podman and check podman version
$ podman run --privileged \
  quay.io/podman/stable \
  podman version

## Run podman inside podman and using ubi8 image inside.
$ podman run --privileged \
  quay.io/podman/stable \
  podman run ubi8 echo hello

$ podman run -it --privileged \
  docker.io/mysticrenji/podman \
  podman version

$ podman run -it --privileged \
  docker.io/mysticrenji/podman \
  podman run -d docker.io/library/node:12-alpine

$ podman run -it --privileged \
  docker.io/mysticrenji/podman \
  podman version && git version uptime\
  uptime;\
  git version;\
  git clone https://github.com/mysticrenji/podman-experiments.git;\
  cd podman-experiments;\
  podman-compose up -d;\
  podman-compose down
  podman images
  
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: podman-in-podman
spec:  # specification of the pod’s contents
  restartPolicy: Never
  containers:
  - name: podipod
    image: "docker.io/mysticrenji/podman"
    command: ["/bin/sh"]
    args:
      - -c
      - >-
          podman version &&
          git clone https://github.com/mysticrenji/podman-experiments.git &&
          cd podman-experiments &&
          podman-compose up -d &&
          podman-compose down &&
          podman images
    securityContext:
       privileged: true
```          

## Podman Machine on MacOS

```shell
## Intall podman and qemu
brew install podman qemu
podman machine init
podman machine start
```

## Reference

- **[Replacing Docker with Podman - Power of Podman](https://cloudnweb.dev/2019/06/replacing-docker-with-podman-power-of-podman/)**
- [Podman on MacOS](https://podman.io/blogs/2021/09/06/podman-on-macs.html)
- [How to use the --privileged flag with container engines](https://www.redhat.com/sysadmin/privileged-flag-container-engines)
- [How to use Podman inside of Kubernetes](https://www.redhat.com/sysadmin/podman-inside-kubernetes)

## Troubleshooting

### Unable to connect to podman - podman machine

```
$ podman version
Cannot connect to Podman. Please verify your connection to the Linux system using `podman system connection list`, or try `podman machine init` and `podman machine start` to manage a new Linux VM
Error: unable to connect to Podman. failed to create sshClient: dial unix /private/tmp/com.apple.launchd.WAh1QMSoLg/Listeners: connect: no such file or directory
```

**Solution**

```shell
$ unset SSH_AUTH_SOCK
```