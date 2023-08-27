---
layout: post
title: Ansible for OpenShift Connection
categories: [ cloud, openshift, containers ]
tags: []
show-avatar: true
permalink: ansible-for-openshift
featured: false
hidden: false
titleshort: Ansible OpenShift
---

OpenShift Credential

Username and password based login for OpenShift

```
---
fields:
  - id: openshift_username
    type: string
    label: Username
  - id: openshift_password
    type: string
    label: Password
    secret: true
required:
  - openshift_username
  - openshift_password
```

```
env:
  OPENSHIFT_LOGIN_PASSWORD: '{{ openshift_password }}'
  OPENSHIFT_LOGIN_USERNAME: '{{ openshift_username }}'

```