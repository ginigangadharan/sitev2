---
layout: post
title: Docker Cheat Sheet
author: gini
categories: [ cheat sheets ]
#image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: docker-cheat-sheet
featured: false
hidden: false
showindex: true
titleshort: docker cheatsheet
---

This is the upstream file for the Docker Cheat Sheet in [techbeatly](https://www.techbeatly.com/)(Yet to publish).

- [Essential Docker Registry Commands](#essential-docker-registry-commands)
  - [Private Docker Registry and Access](#private-docker-registry-and-access)
- [Docker Commands](#docker-commands)
  - [Image Handling](#image-handling)
  - [`docker build`](#docker-build)
  - [Running Containers](#running-containers)
  - [Docker Utilities](#docker-utilities)
  - [Cleaning Docker Environment](#cleaning-docker-environment)
- [Dockerfile](#dockerfile)
- [Advanced Dockerfile Instructions](#advanced-dockerfile-instructions)
  - [The RUN Instruction](#the-run-instruction)
  - [The LABEL Instruction](#the-label-instruction)
  - [The WORKDIR Instruction](#the-workdir-instruction)
  - [The ENV Instruction](#the-env-instruction)
  - [The USER Instruction](#the-user-instruction)
  - [The VOLUME Instruction](#the-volume-instruction)
- [Building Images with the ONBUILD Instruction](#building-images-with-the-onbuild-instruction)
- [Points to Remember](#points-to-remember)

## Essential Docker Registry Commands

```shell
docker login -u USER_NAME -p TOKEN REGISTRY_URL
                                # before we push images, we need to
                                  login to docker registry.

docker login -u developer -p ${TOKEN} \
  docker-registry-default.apps.lab.example.com
                                # TOKEN can be get as TOKEN=$(oc whoami)

docker images --no-trunc --format '{{.ID}} {{.CreatedSince}}' --filter "dangling=true" --filter "before=IMAGE_ID"
                                # list image with format and
                                # using multiple filters
```

### Private Docker Registry and Access
```
kubectl create secret docker-registry private-docker-cred \
    --docker-server=myregistry
    --docker-username=registry-user
    --docker-password=registry-password
    --docker-email=registry-user@example.com
                                # Create a secret for docker-registry
```
Then specify the image pull secret under the `imagePullSecrets` of pod/deployment definition (same level of `container`)
```
    imagePullSecrets:
    - name: private-docker-cred
```

## Docker Commands

### Image Handling

```
docker create [IMAGE]           # Create a new container from a particular image.
docker search [term]            # Search the Docker Hub repository for a particular term.
docker login                    # Log into the Docker Hub repository.
docker pull [IMAGE]             # Pull an image from the Docker Hub repository.
docker push [username/image]    # Push an image to the Docker Hub repository.
docker tag [source] [target]    # Create a target tag or alias that refers to a source image.
```

### `docker build`

```shell
docker build [OPTIONS] PATH
docker build --help

  -t, --tag - set the name and tag of the image
  -f, --file - set the name of the Dockerfile
  --build-arg - set build-time variables
```

### Running Containers

```
docker start [CONTAINER]        # Start a particular container.
docker stop [CONTAINER]         # Stop a particular container.
docker exec -ti [CONTAINER] [command]
                                # Run a shell command inside a particular container.
docker run -ti — image [IMAGE] [CONTAINER] [command]
                                # Create and start a container at the same time, and then run a command inside it.
docker run -ti — rm — image [IMAGE] [CONTAINER] [command]
                                # Create and start a container at the same time,
                                # run a command inside it, and then remove the container
                                # after executing the command.
docker pause [CONTAINER]        # Pause all processes running within a particular container.
```

### Docker Utilities

```
docker history [IMAGE]          # Display the history of a particular image.
docker ps                       # List all of the containers that are currently running.
docker version                  # Display the version of Docker that is currently installed on the system.
docker images                   # List all of the images that are currently stored on the system.
docker inspect [object]         # Display low-level information about a particular Docker object.
```

### Cleaning Docker Environment

```
docker kill [CONTAINER]         # Kill a particular container.
docker kill $(docker ps -q)     # Kill all containers that are currently running.
docker rm [CONTAINER]           # Delete a particular container that is not currently running.
docker rm $(docker ps -a -q)    # Delete all containers that are not currently running.
docker network ls               # list available networks
```

## Dockerfile

FROM -  to set the base image
RUN - to execute a command
COPY & ADD  - to copy files from host to the container
CMD - to set the default command to execute when the container starts
EXPOSE - to expose an application port

## Advanced Dockerfile Instructions

### The RUN Instruction
- `RUN` - executes the commands in a new layer on top of current image and then commits the result. (using `/bin/sh` to execute command)
  ```
  RUN yum --disablerepo=* --enablerepo="rhel-7-server-rpms"
  RUN yum update
  RUN yum install -y httpd
  RUN yum clean all -y
  ```
  - each `RUN` will create and additional layer and effects image size. So the best practice is to combine multiple `RUN` commands together if possible (use `&&`)
```
RUN yum --disablerepo=* --enablerepo="rhel-7-server-rpms" && \
    yum update && \
    yum install -y httpd && \
    yum clean all -y
```

### The LABEL Instruction

- `LABEL` - defines image metadata as key-value pair.
  OpenShift supported labels
  - io.openshift.tags
  - io.k8s.description
  - io.openshift.expose-services
  -
  ```
  LABEL version="2.0" \
      description="This is an example container image" \
      creationDate="01-09-2017"
  ```

### The WORKDIR Instruction
- `WORKDIR` - set the working directory for any following `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD` instructions. Recommended to use absolute path.

### The ENV Instruction
- `ENV` - defines the environment variables inside the container.
  - use `env` command to list environment variables inside a container.

  ```
  ENV MYSQL_DATABASE_USER="my_database_user" \
    MYSQL_DATABASE="my_database"
  ```

### The USER Instruction
- `USER` - run image as a specific user (recommended to run as non-root user). OpenShift will ignore the `USER` instruction and will use a random userid other than root (0).

###  The VOLUME Instruction
- `VOLUME` - crate a mount point inside the container and keep the data persistent.

## Building Images with the ONBUILD Instruction

- use `ONBUILD` to declare instructions that are executed only when building a child image.


```shell
FROM registry.access.redhat.com/rhscl/nodejs-6-rhel7
EXPOSE 3000
# Mandate that all Node.js apps use /usr/src/app as the main folder (APP_ROOT).
RUN mkdir -p /opt/app-root/
WORKDIR /opt/app-root

# Copy the package.json to APP_ROOT
ONBUILD COPY package.json /opt/app-root

# Install the dependencies
ONBUILD RUN npm install

# Copy the app source code to APP_ROOT
ONBUILD COPY src /opt/app-root

# Start node server on port 3000
CMD [ "npm", "start" ]
```

## Points to Remember

- Docker was started as a project by a company called **[dotCloud](https://www.docker.com/docker-news-and-press/dotcloud-inc-now-docker-inc)**, made available as open source in March 2013.
- Kubernetes surfaced from work at Google in 2014, and became the standard way of managing containers.
