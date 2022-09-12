---
layout: postpage
title: ansible-builder
categories: [ Ansible, Automation ]
image: "assets/images/2020/ansible-automation.png"
tags: [ aap, cloud, automation, containers, kubernetes, ansible automation platform, red hat ansible]
permalink: ansible-builder
featured: false
hidden: false
#titleshort: Ansible
---

## Execution Environment

An execution environment is expected to contain:

- Ansible
- Ansible Runner
- Ansible Collections
- Python and/or system dependencies of:
  - modules/plugins in collections
  - content in ansible-base
  - custom user needs

Read [Migrate legacy venvs to execution environments](https://docs.ansible.com/automation-controller/4.2.0/html/upgrade-migration-guide/upgrade_to_ees.html#migrate-new-venv) to learn more.

## Install `ansible-builder`

```shell
$ pip install ansible-builder
```

Refer to the [documentation](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html#install-ansible-builder) for other methods.

## Building custom execution environments with ansible-builder

```shell
ansible-builder --help

--tag       name of the container image
--file      specify the config file
--output-filename 
            Name of file to write image definition to

```

Sample `execution-environment.yml`

```yaml
---
version: 1
dependencies:
  galaxy: requirements.yml
```  

Sample `requirements.yml`

---
collections:
  - name: servicenow.its
```

Build EE images

```shell
ansible-builder build -v3 -t custom-ee
```

- `v3` - add verbosity
- `-t custom-ee` - tag the image name as `custom-ee`
- `--container-runtime` - specify docker or podman

```shell
rhel@ansible project_directory]$ podman images
REPOSITORY                                                               TAG         IMAGE ID      CREATED         SIZE
localhost/custom-ee                                                      latest      8e2411838dd0  15 seconds ago  594 MB
<none>                                                                   <none>      44782f13cb7f  39 seconds ago  602 MB
<none>                                                                   <none>      5b9e207981ac  59 seconds ago  396 MB
registry.redhat.io/ansible-automation-platform-21/ee-supported-rhel8     latest      16674d4e7c5a  3 months ago    1.13 GB
registry.redhat.io/ansible-automation-platform-21/ansible-builder-rhel8  latest      a269ad55a63d  3 months ago    361 MB
registry.redhat.io/ansible-automation-platform-21/ee-minimal-rhel8       latest      9df7b354b3d5  3 months ago    394 MB
```

`execution-environment.yml`

```yaml
---
version: 1
dependencies:
  galaxy: requirements.yml
  python: requirements.txt
  system: bindep.txt
additional_build_steps:
  prepend: |
    RUN whoami
    RUN cat /etc/os-release
  append:
    - RUN echo This is a post-install command!
    - RUN ls -la /etc
```    

`requirements.yml`

```yaml
---
collections:
  - name: servicenow.itsm
  - name: ansible.utils
```

`requirements.txt`

```shell
gcp-cli
ncclient
netaddr
paramiko
```

`bindep.txt`

```shell
findutils [compile platform:centos-8 platform:rhel-8]
gcc [compile platform:centos-8 platform:rhel-8]
make [compile platform:centos-8 platform:rhel-8]
python38-devel [compile platform:centos-8 platform:rhel-8]
python38-cffi [platform:centos-8 platform:rhel-8]
python38-cryptography [platform:centos-8 platform:rhel-8]
python38-pycparser [platform:centos-8 platform:rhel-8]
```

```shell
ansible-builder build -v3 -t custom-ee
```

Check `podman images`

Check `context/Containerfile`

```shell
[rhel@ansible project_directory]$ cat context/Containerfile 
ARG EE_BASE_IMAGE=registry.redhat.io/ansible-automation-platform-21/ee-minimal-rhel8:latest
ARG EE_BUILDER_IMAGE=registry.redhat.io/ansible-automation-platform-21/ansible-builder-rhel8:latest

FROM $EE_BASE_IMAGE as galaxy
ARG ANSIBLE_GALAXY_CLI_COLLECTION_OPTS=
USER root

ADD _build /build
WORKDIR /build

RUN ansible-galaxy role install -r requirements.yml --roles-path /usr/share/ansible/roles
RUN ansible-galaxy collection install $ANSIBLE_GALAXY_CLI_COLLECTION_OPTS -r requirements.yml --collections-path /usr/share/ansible/collections

FROM $EE_BUILDER_IMAGE as builder

COPY --from=galaxy /usr/share/ansible /usr/share/ansible

ADD _build/requirements.txt requirements.txt
ADD _build/bindep.txt bindep.txt
RUN ansible-builder introspect --sanitize --user-pip=requirements.txt --user-bindep=bindep.txt --write-bindep=/tmp/src/bindep.txt --write-pip=/tmp/src/requirements.txt
RUN assemble

FROM $EE_BASE_IMAGE
USER root
RUN whoami
RUN cat /etc/os-release

COPY --from=galaxy /usr/share/ansible /usr/share/ansible

COPY --from=builder /output/ /output/
RUN /output/install-from-bindep && rm -rf /output/wheels
RUN echo This is a post-install command!
RUN ls -la /etc
```

## Build an EE for Network Automation

```yaml
---
version: 1

build_arg_defaults:
  EE_BASE_IMAGE: 'quay.io/ansible/ansible-runner:stable-2.10-devel'
#ARG EE_BUILDER_IMAGE=registry.redhat.io/ansible-automation-platform-21/ansible-builder-rhel8:latest

ansible_config: 'ansible.cfg'

dependencies:
  #galaxy: requirements.yml
  python: requirements.txt
```

## Build using Podman

```shell
podman build -f Containerfile -t <localimagename>:latest .
```

## Resources

- [Execution Environments](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html)
- [Execution Environment Setup Reference](https://docs.ansible.com/automation-controller/latest/html/userguide/ee_reference.html)
- [Ansible Automation Platform supported execution environment](https://catalog.redhat.com/software/containers/ansible-automation-platform-21/ee-supported-rhel8/6177cff2be25a74c009224fa?container-tabs=overview&gti-tabs=red-hat-login)


## Troubleshooting

```
Error: writing blob: storing blob to file "/var/tmp/storage404563786/5": gzip: invalid checksum
```

- move or delete `/var/tmp/storage*`