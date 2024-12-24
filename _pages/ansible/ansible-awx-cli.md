---
layout: post
title: Ansible awx cli
categories: [ ansible ]
tags: [ansible]
show-avatar: false
permalink: awx-cli
featured: false
hidden: false
showindex: true
---

Note: This notes are based on the AAP 2.5

## Installation

```shell
# On Red Hat Enterprise Linux 8, the Controller CLI can be installed via dnf:
dnf install --enablerepo=ansible-automation-platform-2.4-for-rhel-8-x86_64-rpms automation-controller-cli

# On Red Hat Enterprise Linux 9, the Controller CLI can be installed via dnf:
dnf install --enablerepo=ansible-automation-platform-2.5-for-rhel-9-x86_64-rpms automation-controller-cli
```

Setup environment variables (or pass it via CLI)

```shell
export AWXKIT_API_BASE_PATH='/api/controller/'
```

Fetch token from awx cli

```shell
$ awx login \
    --conf.host https://aap25.lab.iamgini.com \
    --conf.username admin \
    --conf.password aapadmin \
    -k
```

You can also the environment variables as follows:

```shell
export AWXKIT_API_BASE_PATH='/api/controller/'
export CONTROLLER_HOST='https://aap25.lab.iamgini.com'
export CONTROLLER_VERIFY_SSL=false
export CONTROLLER_USERNAME=admin
export CONTROLLER_PASSWORD=aapadmin
export CONTROLLER_OAUTH_TOKEN='eRVUrESPle657GUyRVSvd5E0rdJ8VB'
```

Fetch the token and execute the commands.
(You can also fetch token from the [AAP WEBUI](https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.5/html/access_management_and_authentication/gw-token-based-authentication#proc-controller-apps-create-tokens))

```shell
awx --conf.host https://aap25.lab.iamgini.com/ \
    --conf.token 'YOUR-TOKEN-HERE'
    --conf.username admin --conf.password aapadmin \
    -k \
    users list
```


## References

- [How to get the awx CLI to work with AAP 2.5 GA](https://access.redhat.com/solutions/7091403)
- [AWX Command Line Interface](https://docs.ansible.com/automation-controller/latest/html/controllercli/index.html) (v4.5)
