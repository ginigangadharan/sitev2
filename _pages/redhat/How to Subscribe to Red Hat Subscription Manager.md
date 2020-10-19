# How to Subscribe to Red Hat Subscription Manager

```
Register your system to Red Hat Subscription Manager.

[root@host ~]# subscription-manager register
Set a role for your system.

[root@host ~]# subscription-manager role --set="Red Hat Enterprise Linux Server"
Attach your Red Hat Ansible Engine subscription. This command helps you find your Red Hat Ansible Engine subscription:

[root@host ~]# subscription-manager list --available
Use the pool ID of the subscription to attach the pool to the system.

[root@host ~]# subscription-manager attach --pool=<engine-subscription-pool>
Enable the Red Hat Ansible Engine repository.

[root@host ~]# subscription-manager repos \
> --enable ansible-2-for-rhel-8-x86_64-rpms
Install Red Hat Ansible Engine.

[root@host ~]# yum install ansible
If you are using the version with limited support provided with your Red Hat Enterprise Linux subscription, use the following procedure:

Enable the Red Hat Ansible Engine repository.

[root@host ~]# subscription-manager refresh
[root@host ~]# subscription-manager repos \
> --enable ansible-2-for-rhel-8-x86_64-rpms
Install Red Hat Ansible Engine.

[root@host ~]# yum install ansible
```