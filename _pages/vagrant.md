---
layout: post
title: Vagrant Sandbox
categories: [ cloud ]
tags: []
show-avatar: false
permalink: vagrant
featured: false
hidden: false
showindex: true
---

<!-- TOC -->autoauto- [Installing vagrant](#installing-vagrant)auto- [Project Setup](#project-setup)auto- [Boxes](#boxes)auto  - [Manually downloading Boxes](#manually-downloading-boxes)auto  - [List down available boxes](#list-down-available-boxes)auto- [Configure Vagrantfile](#configure-vagrantfile)auto- [Vagrant Up and SSH to your machine](#vagrant-up-and-ssh-to-your-machine)auto- [Connect to your instance](#connect-to-your-instance)auto- [Synced Folders](#synced-folders)auto- [Provisioning](#provisioning)auto  - [Create bootstrap.sh](#create-bootstrapsh)auto- [Networking](#networking)auto  - [Port Forwarding](#port-forwarding)auto  - [Other Networking](#other-networking)auto- [Vagrant Share](#vagrant-share)auto- [Cleanup your vagrant environment](#cleanup-your-vagrant-environment)auto  - [Suspend](#suspend)auto  - [Halt](#halt)auto  - [Destroy](#destroy)auto- [Rebuilding the Environment](#rebuilding-the-environment)auto- [Providers](#providers)autoauto<!-- /TOC -->

## Installing vagrant
Vagrant installation is so easy, just download your package from vagrant portal and install as per instruction. 
Refer [Instalation Doc](https://www.vagrantup.com/docs/installation/).

Verify vagrant installation
```
$ vagrant -v
Vagrant 2.1.1
```

## Project Setup

The purpose of the Vagrantfile:
- Mark the root directory of your project. Many of the configuration options in Vagrant are relative to this root directory.
- Describe the kind of machine and resources you need to run your project, as well as what software to install and how you want to access it.

Create your project directory → goto project directory

```
$ vagrant init
```

It will create a `Vagrantfile` in that directory.	

## Boxes

Instead of building a virtual machine from scratch, which would be a slow and tedious process, Vagrant uses a base image to quickly clone a virtual machine. These base images are known as "boxes" in Vagrant, and specifying the box to use for your Vagrant environment is always the first step after creating a new Vagrantfile.

### Manually downloading Boxes
If you are connected to internet, box images will be downloaded automatically.If you want to manually download box, try below.

Eg: you want to download Ubuntu image `https://app.vagrantup.com/ubuntu/boxes/trusty64/versions/20180611.0.0/`
then append the provider URL and .box name - `https://app.vagrantup.com/<organization-name>/boxes/<box-name>/versions/<version>/providers/<provider>.box`

`https://app.vagrantup.com/ubuntu/boxes/trusty64/versions/20180611.0.0/providers/virtualbox.box`

Then you can add it to Vagrant

```
$ vagrant box add boxes/trusty-server-cloudimg-amd64-vagrant-disk1.box --name ubuntu-trusty64
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'ubuntu-trusty64' (v0) for provider:
	box: Unpacking necessary files from: file://C:/Gini/VM/vagrant/boxes/trusty-server-cloudimg-amd64-vagrant-disk1.box
	box: Progress: 100% (Rate: 267M/s, Estimated time remaining: --:--:--)
==> box: Successfully added box 'ubuntu-trusty64' (v0) for 'virtualbox'!
```

### List down available boxes

```
$ vagrant box list
ubuntu-trusty64 (virtualbox, 0)
```

## Configure Vagrantfile

Now we will configure our Vagrantfile with box name to be used.

```
$ cat Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu-trusty64"
end
```

## Vagrant Up and SSH to your machine
You can start your vagrant instance by vagrant up command.

```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu-trusty64'...
==> default: Matching MAC address for NAT networking...
==> default: Setting the name of the VM: ubuntu-trusty64_default_1529377473485_7675
==> default: Clearing any previously set forwarded ports...
Vagrant is currently configured to create VirtualBox synced folders with
the `SharedFoldersEnableSymlinksCreate` option enabled. If the Vagrant
guest is not trusted, you may want to disable this option. For more
information on this option, please refer to the VirtualBox manual:

  https://www.virtualbox.org/manual/ch04.html#sharedfolders

This option can be disabled globally with an environment variable:

  VAGRANT_DISABLE_VBOXSYMLINKCREATE=1

or on a per folder basis within the Vagrantfile:

  config.vm.synced_folder '/host/path', '/guest/path', SharedFoldersEnableSymlinksCreate: false
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
	default: Adapter 1: nat
==> default: Forwarding ports...
	default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
	default: SSH address: 127.0.0.1:2222
	default: SSH username: vagrant
	default: SSH auth method: private key
	default: Warning: Connection aborted. Retrying...
	default: Warning: Remote connection disconnect. Retrying...
	default: Warning: Connection aborted. Retrying...
	default: Warning: Connection reset. Retrying...
	default: Warning: Connection aborted. Retrying...
	default: Warning: Connection reset. Retrying...
	default: Warning: Connection aborted. Retrying...
	default:
	default: Vagrant insecure key detected. Vagrant will automatically replace
	default: this with a newly generated keypair for better security.
	default:
	default: Inserting generated public key within guest...
	default: Removing insecure key from the guest if it's present...
	default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
	default: The guest additions on this VM do not match the installed version of
	default: VirtualBox! In most cases this is fine, but in rare cases it can
	default: prevent things such as shared folders from working properly. If you see
	default: shared folder errors, please make sure the guest additions within the
	default: virtual machine match the version of VirtualBox you have installed on
	default: your host and reload your VM.
	default:
	default: Guest Additions Version: 4.3.36
	default: VirtualBox Version: 5.2
==> default: Mounting shared folders...
	default: /vagrant => C:/Gini/VM/vagrant/ubuntu-trusty64
```

Our instanance is up and running as you can see above. You can also see the instance details from Oracle VM VirtualBox Manager console.

## Connect to your instance

```
$ vagrant ssh
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.13.0-151-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

 System information disabled due to load higher than 1.0

  Get cloud support with Ubuntu Advantage Cloud Guest:
	http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.
vagrant@vagrant-ubuntu-trusty-64:~$ hostname; uptime; uname -a
vagrant-ubuntu-trusty-64
 03:16:59 up 12 min,  1 user,  load average: 0.00, 0.03, 0.05
Linux vagrant-ubuntu-trusty-64 3.13.0-151-generic #201-Ubuntu SMP Wed May 30 14:22:13 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
vagrant@vagrant-ubuntu-trusty-64:~$
```

This is a fully functions ssh session and you can do anything as in a normal ubuntu machine. (except rm -rf / which will remove the shared folder content as well)

You can logout form the ssh session as normal.

```
vagrant@vagrant-ubuntu-trusty-64:~$ logout
Connection to 127.0.0.1 closed.
```

## Synced Folders
You can access your local files from guest machines (vagrant instances) using synced folders. By default, Vagrant shares your project directory to the /vagrant directory in your guest machine. If you are facing any issues for this, check your VM Guest addition packages and versions. (Can refer our vagrant up console output, you can see the guest addition checks.)

**Let’s try some tasks**

We will check the /vagrant directory and create a new file there.

```
vagrant@vagrant-ubuntu-trusty-64:~$ ls
vagrant@vagrant-ubuntu-trusty-64:~$ cd /vagrant/
vagrant@vagrant-ubuntu-trusty-64:/vagrant$ ls -l
total 1
-rwxrwxrwx 1 vagrant vagrant 75 Jun 19 03:03 Vagrantfile
vagrant@vagrant-ubuntu-trusty-64:/vagrant$ cat >newfile.txt
This is a new file created from vagrant instance.
vagrant@vagrant-ubuntu-trusty-64:/vagrant$ ls -l
total 1
-rwxrwxrwx 1 vagrant vagrant 36 Jun 19 03:44 newfile.txt
-rwxrwxrwx 1 vagrant vagrant 75 Jun 19 03:03 Vagrantfile
vagrant@vagrant-ubuntu-trusty-64:/vagrant$ logout
Connection to 127.0.0.1 closed.
```

**Let’s check this from our host machine now**

```
$ pwd
/c/Gini/VM/vagrant/ubuntu-trusty64
$ ls -l
total 2
-rw-r--r-- 1 TS-AP+gmadappa 4266656257 50 Jun 19 11:45 newfile.txt
-rw-r--r-- 1 TS-AP+gmadappa 4266656257 75 Jun 19 11:03 Vagrantfile
$ cat newfile.txt
This is a new file created from vagrant instance.
```

## Provisioning
Let’s take a scenario where we need this virtual machine serve as a web server. Normally, every time anyone using this machine need to start this vm, login to the guest machine and install these packages and setup this. Since vagrant has automated provisioning features, you can easily configure the bootstrap to install all required packages and setup automatically.

### Create bootstrap.sh

```
$ cat bootstrap.sh

# !/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
Configure bootstrap.sh in Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu-trusty64"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

When you run vagrant again, it will create the vm and bootstrap.sh and you will get a vm with apache installed and webroot configured as you mentioned in the bootstrap.sh file. If your vm is already running, try `vagrant reload --provision.
Note : in below example I have setup nginx server setup. Since I want to test this without internet connection (offline) I have already downloaded the `nginx-1.15.0.tar.gz` file in project directory and this will be available in `/vagrant` shared directory. 
Usually I test those commands manually inside a vm and make sure all are running properly to avoid issues during vagrant up.

```
# !/usr/bin/env bash

cd /vagrant
tar zxvf nginx-1.15.0.tar.gz
cd nginx-1.15.0/
./configure --without-http_rewrite_module --without-http_gzip_module
make
sudo make install
sudo /usr/local/nginx/sbin/nginx
```

(To stop nginx `/usr/bin/nginx -s stop`; where `-s` signal means to send a signal to a master process: stop, quit, reopen, reload)

Let’s test the web server from machine itself (as we did not configure networking for this vm)

```
vagrant@vagrant-ubuntu-trusty-64:~$ wget 127.0.0.1
--2018-06-21 05:30:55--  http://127.0.0.1/
Connecting to 127.0.0.1:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 612 [text/html]
Saving to: 'index.html'

100%[=================================================>] 612     	--.-K/s   in 0s

2018-06-21 05:30:55 (10.2 MB/s) - 'index.html' saved [612/612]
=====================================================================================
```

## Networking

Now we have a web server running inside vm but we need to configure this server to be accessed from outside world (or host machine)

### Port Forwarding
Port forwarding allows you to specify ports on the guest machine to share via a port on the host machine. Add below line to your Vagrantfile

`config.vm.network :forwarded_port, guest: 80, host: 4567`

Then `vagrant reload` and try `http://127.0.0.1:4567` from your host machine and see the webpage is loading. 

### Other Networking

You can also  assign a static IP address to your guest vm or bridge vm onto an existing network.

## Vagrant Share
By using Vagrant Share you can share your Vagrant environment to anyone via Internet by using a URL.
  -  HTTP sharing
  -  SSH sharing - allow instant SSH access by running vagrant connect `--ssh`
  -  General sharing

Run vagrant share

```
$ vagrant share
...
==> default: Creating Vagrant Share session...
==> default: HTTP URL: http://test928923.ngrok.io
...
```

You can use that url (should be something like above) for accessing this. To end the sharing session, hit `Ctrl+C` in your terminal.

Note : This plugin was removed in 2.0.2. If you find the share option is not available, you can install this plugin as show below.

`$ vagrant plugin install vagrant-share`

## Cleanup your vagrant environment

Once you finished, you can resuspend, halt or destroy your vm’s.

### Suspend
vagrant suspend will save the current running state of the machine and stop it. It will still take your disk space but easy to get back online.

```
$ vagrant suspend
==> default: Saving VM state and suspending execution...
```

### Halt
`vagrant halt` will gracefully shutdown and power off the guest vm. This is clean method but still preserve the content of your disks. Since its a full power cycle, vm will take extra time to get online back.

### Destroy
`vagrant destroy` will stop your vm, power it off and remove all the guest related data and disk. your guest vm. When you want machine back, just do a vagrant up. The disk space and RAM consumed by the guest machine is reclaimed and your host machine is left clean. Please note, since you remove all disks and images, it will take extra time to re-import image and make vm online again.

## Rebuilding the Environment
As we learned, even you halt, suspend or destroy you can rebuild the VM’s at anytime by running vagrant up as usual. (Make sure you don’t remove the project directory and Vagrantfile).

## Providers

By default, vagrant is using VirtualBox as provider but you can use VMWare or AWS as well.

```
$ vagrant up --provider=vmware_fusion
or
$ vagrant up --provider=aws
```
