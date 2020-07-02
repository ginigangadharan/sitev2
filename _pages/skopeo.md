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
---

- **[Skopeo Repo](https://github.com/containers/skopeo)**
- **[Say “Hello” to Buildah, Podman, and Skopeo](https://servicesblog.redhat.com/2019/10/09/say-hello-to-buildah-podman-and-skopeo/)** - Red Hat Blog
- **[Promoting container images between registries with skopeo](https://www.openshift.com/blog/promoting-container-images-between-registries-with-skopeo)**
- **[Skopeo Help](https://www.systutorials.com/docs/linux/man/1-skopeo/)**
- **[Skopeo 1.0 released](https://www.redhat.com/en/blog/skopeo-10-released)**

*Note : This is s a living document and I will update whenever needed*

```
skopeo copy \
  oci:/home/USER/SOURCE_PATH \
  docker://quay.io/USER/IMAGE_NAME:VER
                                    # copy local oci image to destination
skopeo copy \
  --dest-creds=USER_NAME:TOKEN \
  oci:/home/USER/SOURCE_PATH \
  docker://REGISTRY_URL/USER/IMAGE_NAME:VER
                                    # copy using credential to destination

skopeo inspect REGISTRY_URL/IMAGE_NAME
                                    # inspect the image

                                    #
```