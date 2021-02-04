---
layout: post
title: GitLab Notes
author: gini
categories: [ cloud ]
tags: [cloud, automation, containers]
permalink: gitlab
featured: false
hidden: false
image: "assets/images/tools-1426927308491-6380b6a9936f.jpeg"
titleshort: Tools
---

## Install GitLab Runner (Binary)

[Reference](https://docs.gitlab.com/runner/install/linux-manually.html)

```bash
# Download binary
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"

# Give it permissions to execute:
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab CI user:
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as service:
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

## Run GitLab Runner in Docker

```bash
docker run -d --name gitlab-runner --restart always \
     -v /srv/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest

sudo docker run -d --name gitlab-runner --restart always \
     -v /home/devops/gitlab-runner/config:/etc/gitlab-runner \
     -v /var/run/docker.sock:/var/run/docker.sock \
     gitlab/gitlab-runner:latest     
```


# References
- [Improve Your Docker Build Time in GitLab CI](https://medium.com/swlh/dramatically-improve-your-docker-build-time-in-gitlab-ci-db0259f1bb08)