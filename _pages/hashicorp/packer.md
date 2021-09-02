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

```shell
$ touch example.pkrvars.hcl

docker_image = "ubuntu:bionic"

# build with varible file
$ packer build --var-file=example.pkrvars.hcl docker-ubuntu.pkr.hcl
```

Packer will automatically load any variable file that matches the name `*.auto.pkrvars.hcl`, without the need to pass the file via the command line.

**Build image with command line flag**

```shell
$ packer build --var docker_image=ubuntu:groovy .
```

# Packer Parallel Build

Add multiple sources

```json
source "docker" "ubuntu" {
  image  = var.docker_image
  commit = true
}

source "docker" "ubuntu-bionic" {
  image  = "ubuntu:bionic"
  commit = true
}
```

Add multiple sources in build

```json
build {
  name    = "learn-packer"
  sources = [
    "source.docker.ubuntu",
    "source.docker.ubuntu-bionic",
  ]
.
.
```

# Packer Post-Processors

While provisioners are run against an instance while it is running, post-processors run only after Packer saves the instance as an image.

eg:

- compress artifacts
- upload artifacts
- create some files

**Adding a docker tag using post-processor**

Post
```json
build {
  name    = "learn-packer"
  .
  .
  post-processor "docker-tag" {
    repository = "learn-packer"
    tags       = ["ubuntu-xenial", "packer-rocks"]
    only       = ["docker.ubuntu"]
  }

  post-processor "docker-tag" {
    repository = "learn-packer"
    tags       = ["ubuntu-bionic", "packer-rocks"]
    only       = ["docker.ubuntu-bionic"]
  }
```

**Sequential post-processing steps**

- Use `post-processors` instead of `post-processor`
- Use the post-processors (note the pluralization) block to create post-processing pipelines where the output of one post-processor becomes the input to another post-processor.

```json
  post-processors {
    post-processor "docker-import" {
      repository = "swampdragons/testpush"
      tag        = "0.7"
    }
    post-processor "docker-push" {}
  }
```

# Packer with AWS

## Manage AWS Credentials for Packer

Configure environment variables.

```shell
$ export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
$ export AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
```


