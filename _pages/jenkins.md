---
layout: post
title: Jenkins
author: gini
categories: [ automation, cicd ]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: jenkins
featured: false
hidden: false
showindex: true
titleshort: jenkins
---

- [Start Jenkins inside a container](#start-jenkins-inside-a-container)
  - [Run `jenkins` as a docker container](#run-jenkins-as-a-docker-container)
  - [Get the Password](#get-the-password)
  - [Login to Jenkins GUI for the first time](#login-to-jenkins-gui-for-the-first-time)
  - [Sample docker-compose.yml](#sample-docker-composeyml)
- [Create Multibranch Pipeline with Git](#create-multibranch-pipeline-with-git)
- [Reference](#reference)

# Start Jenkins inside a container

Refer **[Docker Hub](https://hub.docker.com/r/jenkins/jenkins/)**
## Run `jenkins` as a docker container

```
docker run \
  --name jenkins \
  --detach \
  --volume jenkins-data:/var/jenkins_home \
  --publish 8080:8080 \
  --publish 50000:5000 \
  jenkins/jenkins:lts
```  

where,
- `--name` - name of the container
- `--detach` or `-d` - detached mode (in background)
- `--volume jenkins-data:/var/jenkins_home` - bind named volume (will create a directory in `/var/lib/docker/volumes/`)
- `--publish 8080:8080`  or `-p` - map or export host port `8080` to container port `8080`
- `jenkins/jenkins:lts` - docker image to be used.
## Get the Password

- First Admin password can be found at `/var/jenkins_home/secrets/initialAdminPassword`
- 
```
$ sudo docker exec -it 198b7deb5f7d /bin/bash
jenkins@198b7deb5f7d:/$ cat /var/jenkins_home/secrets/initialAdminPassword
8d53672878b24941a1cdf3df3c8ec8cc
jenkins@198b7deb5f7d:/$ exit
exit
```

- or check `docker logs CONTAINER_ID` and get the password.   

## Login to Jenkins GUI for the first time 

- Open a web browser and goto `localhost:8080`
- enter the password collected from previous step.
- Install Suggested plugins
- Create the first time user
- confirm jenkins url

## Sample docker-compose.yml

```
version: '3'
services:
  jenkins:
    tty: true
    stdin_open: true
    container_name: jenkins
    image: jenkins/jenkins
    ports:
      - "8080:8080"
    volumes:
      - "$PWD/jenkins_home:/var/jenkins_home"
    networks:
      - net
networks:
 net:
```

# Create Multibranch Pipeline with Git

# Reference

- [Deploying Jenkins X in Linode Kubernetes Engine](https://www.linode.com/docs/kubernetes/how-to-deploy-jenkins-x-in-linode-kubernetes-engine/)
- [Installing Jenkins X using JX Boot (The Non CJXD edition)](http://sharepointoscar.com/2020-01-10-Installing-Jenkins-X/)
- [A collection of tutorials with JX](https://github.com/jenkins-x/jx-tutorial)
- [salaboy/products-service](https://github.com/salaboy/products-service)
- [salaboy/online-store](https://github.com/salaboy/online-store)
- [salaboy/customers-service](https://github.com/salaboy/customers-service)
- [cd.foundation](https://cd.foundation/)