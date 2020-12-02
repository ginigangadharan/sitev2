---
layout: post
title: Jenkins
author: gini
categories: [ automation, cicd ]
#image: "assets/images/how-to-create-scheduled-snapshots-in-google-cloud-platform.PNG"
tags: [cloud, automation, containers, kubernetes]
permalink: jenkins
featured: false
hidden: false
showindex: true
titleshort: jenkins
---

- [Start Jenkin inside a container](#start-jenkin-inside-a-container)
- [Sample docker-compose.yml](#sample-docker-composeyml)
- [Reference](#reference)

# Start Jenkin inside a container

1. Run `jenkins` as a docker container

```
docker run \
  --name jenkins-docker \
  --detach \
  --privileged \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  --publish 8080:8080 \
  --publish 50000:5000 \
  jenkins/jenkins:lts
```  

2. Get the Password

First Admin password can be found at `/var/jenkins_home/secrets/initialAdminPassword`
or 
check `docker logs CONTAINER_ID` and get the password.   

3. Login to Jenkins GUI for the first time 

- Open a web browser and goto `localhost:8080`
- enter the password collected from previous step.


# Sample docker-compose.yml

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

# Reference

- [Deploying Jenkins X in Linode Kubernetes Engine](https://www.linode.com/docs/kubernetes/how-to-deploy-jenkins-x-in-linode-kubernetes-engine/)
- [Installing Jenkins X using JX Boot (The Non CJXD edition)](http://sharepointoscar.com/2020-01-10-Installing-Jenkins-X/)
- [A collection of tutorials with JX](https://github.com/jenkins-x/jx-tutorial)
- [salaboy/products-service](https://github.com/salaboy/products-service)
- [salaboy/online-store](https://github.com/salaboy/online-store)
- [salaboy/customers-service](https://github.com/salaboy/customers-service)
- [cd.foundation](https://cd.foundation/)