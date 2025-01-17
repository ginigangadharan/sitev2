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
# or
export CONTROLLER_OAUTH_TOKEN=$(awx login \
    --conf.host https://aap25.lab.iamgini.com \
    --conf.username admin \
    --conf.password aapadmin \
    -k | jq -r '.token')
# or, using environment variable
export CONTROLLER_OAUTH_TOKEN=$(awx login -k | jq -r '.token')
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

## Gateway API

NOTE: awx is not supporting the gateway and we need to use alternative cli methods.

### Fetching Token

- In the AAP UI, Goto `Access Management` -> `OAuth Applications` -> Create a new `OAuth application` (Authorization grant type: Authorization code)
- Goto API ($CONTROLLER_HOST/api/gateway/v1/tokens/) -> Select the application (you created from UI) and request token. (You can also do the same via CLI using curl command)
- Note down the token and use it for authentication.

### Set the Gateway token

```shell
$ export GATEWAY_OAUTH_TOKEN='WFawgvtSCQkXS7Owi2paPfMaEwjVYc'
```

Examples:

Show Authenticator List with the type `ansible_base.authentication.authenticator_plugins.ldap`

```shell
curl -s -k \
  -H "Authorization: Bearer $GATEWAY_OAUTH_TOKEN" \
  -H "Content-Type: application/json" \
  "$CONTROLLER_HOST/api/gateway/v1/authenticators/" | \
  jq '.results[] | select(.type == "ansible_base.authentication.authenticator_plugins.ldap")'
```

Ensure LDAP is configured with `ldaps`

```shell
$ curl -s -k \
  -H "Authorization: Bearer $GATEWAY_OAUTH_TOKEN" \
  -H "Content-Type: application/json" \
  "$CONTROLLER_HOST/api/gateway/v1/authenticators/" | \
  jq '[.results[] | select(.type == "ansible_base.authentication.authenticator_plugins.ldap")] | any(.configuration.SERVER_URI[]? | test("ldaps:"))'
```

Check if logo or Custom login info

```shell
$ curl -s -k   -H "Authorization: Bearer $GATEWAY_OAUTH_TOKEN"   -H "Content-Type: application/json"   "$CONTROLLER_HOST/api/gateway/v1/settings/all/" |   jq -r '.custom_login_info'
```


## References

- [AWX Command Line Interface](https://docs.ansible.com/automation-controller/latest/html/controllercli/index.html?extIdCarryOver=true&intcmp=7015Y000003t7aWQAQ&sc_cid=RHCTG0240000434975)
- [How to get the awx CLI to work with AAP 2.5 GA](https://access.redhat.com/solutions/7091403)
- [AWX Command Line Interface](https://docs.ansible.com/automation-controller/latest/html/controllercli/index.html) (v4.5)
