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
- [Creating new container image with `podman commit`](#creating-new-container-image-with-podman-commit)
- [Prerequisites](#prerequisites)
  - [1. Pull the Existing Container Image](#1-pull-the-existing-container-image)
  - [Run a Container Using the Pulled Image](#run-a-container-using-the-pulled-image)
  - [Copy Files Into the Container](#copy-files-into-the-container)
  - [Verify the Files Inside the Container](#verify-the-files-inside-the-container)
  - [Commit the Container as a New Image](#commit-the-container-as-a-new-image)
  - [Tag the New Image](#tag-the-new-image)
  - [Push the Image to the Registry](#push-the-image-to-the-registry)
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

## Creating new container image with `podman commit`

> **Note:**
We are using the `podman commit` method instead of `ansible-builder` because this is a disconnected environment.
In such scenarios, the automation platform might be inside an air-gapped environment or restricted network, leading to limitations such as:
> - No access to external Python repositories or internal PyPI repository servers.
> - No access to required RPM repositories.
>
> To build and use custom execution environments with Ansible Automation Platform in such cases, you typically have two options:
> 1. Building and transferring the container image.
> 2. Creating a custom environment.
>
> However, both methods require a connected machine with `podman` installed to build the execution environment.
>
> Given these constraints, we opted for the `podman commit` method as a practical workaround.


This guide walks through pulling a container image, running a container, copying files, verifying them, committing the container as a new image, tagging it, and pushing it to a registry.

## Prerequisites
- Ensure Podman is installed on your system.
- Verify access to the container registry.
- Use `--tls-verify=false` to avoid certificate validation issues with the registry.


### 1. Pull the Existing Container Image

Pull the container image from the registry:

```bash
$ podman pull --tls-verify=false automation.example.com/ee
```

Note: Using --tls-verify=false is only recommended for non-production setups to avoid certificate validation errors. For production, ensure proper certificates are in place.

### Run a Container Using the Pulled Image

Run a new container from the image:

```bash
$ podman run -dit --name ee-container automation.example.com/ee
```

- `-d`: Run the container in detached mode.
- `-i`: Keep STDIN open.
- `-t`: Allocate a pseudo-TTY.
- `--name ee-container`: Assign a name to the container.

### Copy Files Into the Container

Copy the folder and files to the container:

```bash
$ podman cp /apps/scripts/common/ ee-container:/apps/scripts/common/
```

### Verify the Files Inside the Container

Check if the files are correctly copied by executing a command inside the container:

```bash
$ podman exec -it ee-container ls -l /apps/scripts/common/
```

- `podman exec`: Run a command in a running container.
- `-it`: Interactive mode with TTY.
- `ls -l`: Lists files in the specified directory.

### Commit the Container as a New Image

Commit the changes in the container to create a new image:

```bash
$ podman commit ee-container automation.example.com/ee-libs:1.0
```

### Tag the New Image

Tag the new image for the registry:

```bash
$ podman tag automation.example.com/ee-libs:1.0 automation.example.com/ee-libs:1.0
```

### Push the Image to the Registry

Push the new image to the registry:

```bash
$ podman push --tls-verify=false automation.example.com/ee-libs:1.0
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