# Install gogs in docker

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

devops@cloudshell:~$ gcloud auth configure-docker
gcloud credential helpers already registered correctly.
devops@cloudshell:~$ docker pull gogs/gogs
Using default tag: latest
latest: Pulling from gogs/gogs
9d48c3bd43c5: Pull complete
1013a6a6073e: Pull complete
f074cb3ecafc: Pull complete
33433d770f0d: Pull complete
b464f169dfd9: Pull complete
77e3c5692f7d: Pull complete
e6d8ab097b45: Pull complete
13266a8e4353: Pull complete
2ac951b0051b: Pull complete
266414b76811: Pull complete
Digest: sha256:48cd3d14f6d5c9f73cd462b42e67f7a021b299da8fdaa2003cc164fe6ed08a38
Status: Downloaded newer image for gogs/gogs:latest
docker.io/gogs/gogs:latest

devops@cloudshell:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
gogs/gogs           latest              9dee4424cf01        2 weeks ago         99.8MB


Tag the local image with the registry name
Determine the registry name:

Choose a hostname, which specifies the region of the registry's storage. This region is not your location, but the location where the images will be stored. For example, if you are in Australia, you may want to use the host in Asia.

The four options are:

gcr.io hosts the images in the United States, but the location may change in the future
us.gcr.io hosts the image in the United States, in a separate storage bucket from images hosted by gcr.io
eu.gcr.io hosts the images in the European Union
asia.gcr.io hosts the images in Asia
In the console, the images' hostname will be listed under Location.

Choose an image name, which can be different from the image's name on your local machine.

Combine the hostname, your Google Cloud Platform Console project ID, and image name:

[HOSTNAME]/[PROJECT-ID]/[IMAGE]
If your project ID contains a colon (:), see Domain-scoped projects.


devops@cloudshell:~$ docker tag gogs/gogs gcr.io/devops-vinmel/gogs:latest
devops@cloudshell:~$ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
gogs/gogs                   latest              9dee4424cf01        2 weeks ago         99.8MB
gcr.io/devops-vinmel/gogs   latest              9dee4424cf01        2 weeks ago         99.8MB

devops@cloudshell:~$ docker push gcr.io/devops-vinmel/gogs:latest
The push refers to repository [gcr.io/devops-vinmel/gogs]
472aa1d10b15: Pushed
1a0a9e02ec1d: Pushed
cda4f57c1811: Pushed
074fc7d49e95: Pushed
0f12e9fa2e77: Pushed
b8d11cf1485c: Pushed
e18f9d09a6f5: Pushed
b2f817b146eb: Pushed
acf16385892f: Pushed
03901b4a2ea8: Layer already exists
latest: digest: sha256:48cd3d14f6d5c9f73cd462b42e67f7a021b299da8fdaa2003cc164fe6ed08a38 size: 2415
devops@cloudshell:~$ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
gogs/gogs                   latest              9dee4424cf01        2 weeks ago         99.8MB
gcr.io/devops-vinmel/gogs   latest              9dee4424cf01        2 weeks ago         99.8MB


remove:
devops@cloudshell:~$ docker image rm gcr.io/devops-vinmel/gogs
Untagged: gcr.io/devops-vinmel/gogs:latest
Untagged: gcr.io/devops-vinmel/gogs@sha256:48cd3d14f6d5c9f73cd462b42e67f7a021b299da8fdaa2003cc164fe6ed08a38
