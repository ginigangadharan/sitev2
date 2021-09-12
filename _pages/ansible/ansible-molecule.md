---
layout: post
title: Molecule for Testing Ansible Roles
categories: [ Ansible, Automation ]
image: "assets/images/2021/pexels-chokniti-khongchum-2280571-test-ansible.jpg"
tags: [cloud, automation, containers, kubernetes, ansible automation platform, red hat ansible]
permalink: molecule
featured: false
hidden: false
titleshort: molecule
---

# Setting up your environment

## Python Virtual Environment (Optional)

Note: I am using Python Virtual environment for my Ansible Development and testing which is safe and I can test multiple versions of Ansible without breaking exisitng setup.

Read my post [How to set up and use Python virtual environments for Ansible](https://www.redhat.com/sysadmin/python-venv-ansible) in Red Hat Sysadmin blog.

## Configure Podman for Mac (Optional)

This is for MacOS as podman is not natively work on Mac but you can use podman with a virtual machine or remote VM.

```shell
# Install podman
$ brew install podman

# start the Podman-managed VM
podman machine init
podman machine start

# check status
podman info
```

- [Using podman-machine on MacOS](https://github.com/containers/podman/blob/main/docs/tutorials/mac_experimental.md)
- [Podman Installation Instructions](https://podman.io/getting-started/installation)


# Installing Molecule

```shell
$ pip install molecule

$ molecule --version
molecule 3.4.0 using python 3.7 
    ansible:2.9.0
    delegated:3.4.0 from molecule
```

# Molecule Drivers

Molecule uses drivers to bring up Ansible ready hosts to operate on. Currently, Molecule supports three drivers: Vagrant, Docker, and OpenStack. The driver is set to `vagrant` by default if the `--docker` flag is not passed when molecule init is run.

Make sure you have proper plugin or tools installed to use these drivers. For example, to use vagrant as driver, install the appropriate python library.

```shell
$ pip install python-vagrant
$ pip install molecule-vagrant

# Install docker library if you are using podman
$ pip install "molecule[docker]" 

# Install podman library if you are using podman
$ pip install "molecule[vagrant]" 

# Install podman library if you are using podman
$ pip install "molecule[podman]" 

$ pip install "molecule[lint]"
```

# Create a new Role with Molecule

```shell
# init role with podman driver
molecule init role newrole --driver-name=podman

# init role without a driver
$ molecule init role ginigangadharan.hello-demo            
INFO     Initializing new role ginigangadharan.hello-demo...
INFO     Initialized role in /Users/gini/workarea/ansible-collection-custom-modules/roles/ginigangadharan.hello-demo successfully.

$ ls -l ginigangadharan.hello-demo/molecule/default 
total 48
-rw-r--r--  1 gini  staff   280 10 Sep 15:14 INSTALL.rst
-rw-r--r--  1 gini  staff   155 10 Sep 15:14 converge.yml
-rw-r--r--  1 gini  staff  1066 10 Sep 15:14 create.yml
-rw-r--r--  1 gini  staff   567 10 Sep 15:14 destroy.yml
-rw-r--r--  1 gini  staff   142 10 Sep 15:14 molecule.yml
-rw-r--r--  1 gini  staff   177 10 Sep 15:14 verify.yml
```

When you execute above command,

- 'ansible-galaxy` command will be used in the backend.
- Add a molecule directory inside the role
- Configure the role to run tests in a vagrant environment

## Update your molecule.yml

```yaml
---
dependency:
  name: galaxy
driver:
  name: vagrant
platforms:
  - name: instance
    box: fedora/32-cloud-base
    memory: 512
    cpus: 1
provisioner:
  name: ansible
verifier:
  name: ansible
```

# References

- [Testing your Ansible roles with Molecule](https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule)
- [Continuous Testing with Molecule, Ansible, and GitHub Actions](https://www.jeffgeerling.com/blog/2020/continuous-testing-molecule-ansible-and-github-actions)
- [Molecules - Ansible for DevOps by Jeff Geerling](https://github.com/geerlingguy/ansible-for-devops/tree/master/molecule)
- [Setting up Molecule for testing Ansible roles with Vagrant and Testinfra](https://floatingpoint.sorint.it/blog/post/setting-up-molecule-for-testing-ansible-roles-with-vagrant-and-testinfra)