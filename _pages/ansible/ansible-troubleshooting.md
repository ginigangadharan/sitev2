# 1. Troubleshooting Ansible

<!-- TOC orderedlist:true -->

- [1. Troubleshooting Ansible](#1-troubleshooting-ansible)
  - [1.1. Error with Self Signed SSL Cert on SCM server](#11-error-with-self-signed-ssl-cert-on-scm-server)
    - [1.1.1. Resolution](#111-resolution)

<!-- /TOC -->

## 1.1. Error with Self Signed SSL Cert on SCM server

Error : Peer's certificate issuer has been marked as not trusted by the user

```
{
    "stderr_lines": [
        "fatal: unable to access 'https://$encrypted$:$encrypted$@vm-gitnode-01.lab.local/ansible/network-automation.git/': Peer's certificate issuer has been marked as not trusted by the user."
    ],
    "cmd": "/usr/bin/git clone --origin origin 'https://$encrypted$:$encrypted$@vm-gitnode-01.lab.local/ansible/network-automation.git' /var/lib/awx/projects/_8__network_poc",
    "_ansible_no_log": false,
    "stdout": "Cloning into '/var/lib/awx/projects/_8__network_poc'...\n",
    "changed": false,
    "invocation": {
        "module_args": {
            "force": false,
            "track_submodules": false,
            "reference": null,
            "dest": "/var/lib/awx/projects/_8__network_poc",
            "umask": null,
            "clone": true,
            "gpg_whitelist": [],
            "accept_hostkey": false,
            "update": true,
            "ssh_opts": null,
            "repo": "https://$encrypted$:$encrypted$@vm-gitnode-01.lab.local/ansible/network-automation.git",
            "bare": false,
            "archive": null,
            "refspec": null,
            "executable": null,
            "remote": "origin",
            "recursive": true,
            "separate_git_dir": null,
            "verify_commit": false,
            "depth": null,
            "version": "HEAD",
            "key_file": null
        }
    },
    "stderr": "fatal: unable to access 'https://$encrypted$:$encrypted$@vm-gitnode-01.lab.local/ansible/network-automation.git/': Peer's certificate issuer has been marked as not trusted by the user.\n",
    "rc": 128,
    "_ansible_delegated_vars": {
        "ansible_host": "localhost"
    },
    "stdout_lines": [
        "Cloning into '/var/lib/awx/projects/_8__network_poc'..."
    ],
    "msg": "fatal: unable to access 'https://$encrypted$:$encrypted$@vm-gitnode-01.lab.local/ansible/network-automation.git/': Peer's certificate issuer has been marked as not trusted by the user."
}
```

### 1.1.1. Resolution
Resolution
SSL certificate validation can be prevented for Git connections originating from Tower by adding the following settings in the Tower UI at Settings >> Configure Tower >> Jobs in JSON format:

```
{
  "GIT_SSL_NO_VERIFY": "True"
}
```

[Reference](https://access.redhat.com/solutions/3131461)

Note : For cloning repo, use below methods
```
git config --global http.sslverify false
#or
export GIT_SSL_NO_VERIFY=true
```
