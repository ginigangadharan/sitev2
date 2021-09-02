---
layout: post
title: Packer by Hashicorp
author: gini
categories: [ iac, cloud ]
image: "assets/images/2020/infrastructure.jpg"
tags: [iac, packer]
permalink: packer
featured: false
hidden: false
titleshort: packer
---

Packer use cases and samples are kept in this [GitHub](https://github.com/ginigangadharan/packer-usecases) Repo.

# Introduction

Packer is an open source tool for creating identical machine images for multiple platforms from a single source configuration.

# Installing Packer

[Download](https://www.packer.io/downloads) and install packer.

```shell
# packer on linux
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install packer
```

# Create your first Packer file

Sample `docker-ubuntu.pkr.hcl`

```json
packer {
  required_plugins {
    docker = {
      version = ">= 0.0.7"
      source = "github.com/hashicorp/docker"
    }
  }
}

source "docker" "ubuntu" {
  image  = "ubuntu:xenial"
  commit = true
}

build {
  name    = "learn-packer"
  sources = [
    "source.docker.ubuntu"
  ]
}
```

## Init and Build Packer

```shell
# init packer
$ packer init .

# format packer files
$ packer fmt .

# validate packer files
$ packer validate .

# build
$ packer build .
# or
$ packer build docker-ubuntu.pkr.hcl
```

## Adding provisioner

```json
build {
  name = "learn-packer"
  sources = [
    "source.docker.ubuntu"
  ]
  provisioner "shell" {
    environment_vars = [
      "FOO=hello world",
    ]
    inline = [
      "echo Adding file to Docker Container",
      "echo \"FOO is $FOO\" > example.txt",
    ]
  }
  provisioner "shell" {
    inline = ["echo This provisioner runs last"]
  }
}
```

## Variables

```json
variable "docker_image" {
  type    = string
  default = "ubuntu:xenial"
}
```

