# Cloud Init


https://launchpad.net/cloud-init/+download

wget https://launchpad.net/cloud-init/trunk/19.4/+download/cloud-init-19.4.tar.gz
(or curl -LO)

wget https://rpmfind.net/linux/centos/7.7.1908/os/x86_64/Packages/cloud-init-18.5-3.el7.centos.x86_64.rpm



# Install pip

```
# curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
# python get-pip.py 
pip install requests urllib3 pyOpenSSL --force --upgrade
```

write_files:
  - content: |
      This VM was provisioned by Ansible
    path: /root/readme.txt
    permissions: '0644'

[DVD-ISO]
name=DVD-DVD
baseurl=file:///mnt/
gpgcheck=0