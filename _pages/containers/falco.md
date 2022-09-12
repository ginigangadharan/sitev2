---
layout: post
title: Falco - Open source container security
categories: [ container, docker, kubernetes ]
#image: "assets/images/2020/ansible-automation.png"
tags: [cloud, openshift, containers, kubernetes, devops]
permalink: falco
author: gini
featured: false
hidden: false
titleshort: falco
---

- [Runtime Security](#runtime-security)
- [Installing Falco](#installing-falco)
  - [Running Falco in Docker](#running-falco-in-docker)
- [Falco Installation with Kubernetes](#falco-installation-with-kubernetes)
- [Falco Rules](#falco-rules)
- [Falco macros](#falco-macros)
- [Falco lists](#falco-lists)
- [Falco default rules](#falco-default-rules)
- [Override macros](#override-macros)
- [Falco Rules Deep Dive](#falco-rules-deep-dive)
- [Disable default rules](#disable-default-rules)
  - [Disable rules using arguments](#disable-rules-using-arguments)
  - [Disable rules using Macros](#disable-rules-using-macros)
  - [Disable rules using Custom Rule Definition](#disable-rules-using-custom-rule-definition)
- [Rule Conditions](#rule-conditions)
- [Falco Rule Exceptions](#falco-rule-exceptions)
- [Exception Syntax shortcuts](#exception-syntax-shortcuts)
- [Faclo Alerts](#faclo-alerts)
- [Falco exporter](#falco-exporter)
- [Falco Response Engine](#falco-response-engine)
- [References](#references)

## Runtime Security

- Directories
- Previlege escalation
- System binary drifts

## Installing Falco

(Ubuntu)

Refere [documentation](https://falco.org/docs/getting-started/installation)

```shell
curl -s https://falco.org/repo/falcosecurity-3672BA8F.asc | apt-key add -
echo "deb https://download.falco.org/packages/deb stable main" \ | tee -a /etc/apt/sources.list.d/falcosecurity.list

uname -r
apt search linux-headers-$(uname -r)
```

Falco needs a driver (the kernel module or the eBPF probe) to work; if a prebuilt driver is not available for your distribution/kernel, Falco needs kernel headers installed on the host to build the driver on the fly.

```shell
apt-get update && apt-get install falco -y
systemctl start falco
systemctl status falco
```

### Running Falco in Docker

Even using container images, Falco needs kernel headers installed on the host, to correctly build the driver on the fly: the kernel module or the eBPF probe.

```shell
## run container with driver
docker pull falcosecurity/falco-driver-loader:latest
docker run --rm -i -t \
    --privileged \
    -v /root/.falco:/root/.falco \
    -v /proc:/host/proc:ro \
    -v /boot:/host/boot:ro \
    -v /lib/modules:/host/lib/modules:ro \
    -v /usr:/host/usr:ro \
    -v /etc:/host/etc:ro \
    falcosecurity/falco-driver-loader:latest && echo "Falco drivers installed!"

## run container without driver
docker pull falcosecurity/falco-no-driver:latest
docker run --rm -d \
  --name "falco_training" \
  --security-opt apparmor:unconfined \
  -p 8765:8765 \
  -e HOST_ROOT=/ \
  --cap-add SYS_PTRACE \
  --pid=host $(ls /dev/falco* | xargs -I {} echo --device {}) \
  -v /var/run/docker.sock:/var/run/docker.sock \
  falcosecurity/falco-no-driver:latest
```

Falco has a webserver that captures K8S events, review the status of the Falco web server with:

```shell
curl localhost:8765/healthz; echo
```

Trigger one of the Falco rules by executing below command:

```shell
find /root -name "id_rsa"
```

Check logs to see that Falco correctly intercepted the potentially dangerous command:

```shell
docker logs falco_training 2>&1 | grep "find /root -name id_rsa"
```

## Falco Installation with Kubernetes

[Kubernetes Security using Falco](https://falco.org/blog/intro-k8s-security-monitoring/)

```shell
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm repo update
helm install falco falcosecurity/falco

## or
helm install falco \
  --create-namespace \
  --namespace falco \
  --set containerd.enabled=true \
  --set containerd.socket=/run/k3s/containerd/containerd.sock \
  falcosecurity/falco


## check pods
kubectl get pods
container=$(kubectl get po | grep falco | cut -c 1-11)

## Check status in logs
kubectl logs "$container" | grep -A 5 Success

## check health status from container
kubectl exec "$container" -- curl -s localhost:8765/healthz; echo

## execute a critical command and check logs
find /root -name "id_rsa"
## then check Falco logs:
kubectl logs "$container" | grep Warning
```

Install with eBPG alternative

```shell
helm install falco \
  --create-namespace \
  --namespace falco \
  --set containerd.enabled=true \
  --set containerd.socket=/run/k3s/containerd/containerd.sock \
  --set ebpf.enabled=true \
  falcosecurity/falco
```

## Falco Rules

- `/etc/falco/falco_rules.yaml` - default rule
- `/etc/falco/falco_rules.local.yaml` - local rule

rule: a condition under which an alert should be generated, and the output string to send with the alert.
macro: rule condition snippet that can be re-used inside rules and other macros.
list: items that can be included in rules, macros, or other lists.

A Falco rule must contain the following fields (required):

- `rule`: A short, unique name for the rule.
- `condition`: A filtering expression that is applied against events to check whether they match the rule.
- `desc`: A longer description of what the rule detects.
- `output`: Message to output if a matching event occurs. See output for more details.
- `priority`: The severity of the event (for example, critical, error, and warning).

eg:

```yaml
- rule: shell_in_container
  desc: notice shell activity within a container
  condition: container.id != host and proc.name = bash
  output: shell in a container (user=%user.name container_name=%container.name)
  priority: warning
  tags: [shell, container]
```

Custom rule 

```yaml
customRules:
  my_rules: |-
    - rule: Unauthorized mount process
      desc: There is an unauthorized mount process running
      condition: evt.type=execve and proc.name=mount
      output: Unauthorized process (%proc.cmdline) running
      priority: WARNING
      tags: [process, mount]
```

Update deployment with rule file

```shell
helm upgrade falco falcosecurity/falco -n falco -f my_rules.yaml
```

## Falco macros

- `macro`: Name of the macro.
- `condition`: Filtering expression, same as used in rules.

```yaml
- macro: in_container
  condition: container.id != host

- macro: spawned_shell
  condition: proc.name = bash

- rule: shell_in_container
  desc: notice shell activity within a container
  condition: spawned_shell and in_container
  output: shell in a container (user=%user.name ...)
  priority: WARNING
```

## Falco lists

Named collections of items that you can include in rules, macros, or even other lists.

- `list`: Name of the List.
- `items`: List of values (inside square brackets and separated by comma).

```yaml
customRules:
  my_rules: |-
    - rule: Unauthorized mount process
      desc: There is an unauthorized mount process running
      condition: mount_process
      output: Unauthorized process (%proc.cmdline) running
      priority: WARNING
      tags: [process, mount]

    - macro: mount_process
      condition: evt.type=execve and proc.name in (forbidden_processes)

    - list: forbidden_processes
      items: [mount, sudo, su]
```

## Falco default rules


`falco -L` - show the name and description of all rules.

To run inside pod
```shell
FALCO_POD=$(kubectl get pods -n falco | sed -n -e 's/^\(falco-[[:alnum:]]*\).*/\1/p')
kubectl exec ${FALCO_POD} -n falco -- falco -L
```

Appending list

```yaml
- list: shell_libraries
  append: true
  items: [pdksh, fish]
```  

Eg: exisitng default rule.
```yaml
- rule: Create Symlink Over Sensitive Files
  condition: >
    create_symlink and
    (evt.arg.target in (sensitive_file_names) or evt.arg.target in (sensitive_directory_names))
  ...

- list: sensitive_directory_names
  items: [/, /etc, /etc/, /root, /root/]

- list: sensitive_file_names
  items: [/etc/shadow, /etc/sudoers, /etc/pam.conf, /etc/security/pwquality.conf]
```

Append the list in custom rule file

```yaml
  - list: sensitive_directory_names
    append: true
    items: [/mnt]
```

## Override macros

in falco_rules.yaml:

```yaml
- macro: user_trusted_containers
  condition: (never_true)
```

Overriding macro in falco_rules.local.yaml:

```yaml
- macro: user_trusted_containers
  condition: (container.image startswith sysdig/agent)
```

Another example

```yaml
- macro: allowed_ssh_hosts
  condition: fd.sip="10.42.0.1" or fd.sip="10.132.0.48"
```

## Falco Rules Deep Dive

**Falco Engine Versioning**

- `required_engine_version` setting to the rules file to specify the minimum engine version required to read a rules file.

```shell
$ kubectl exec -n falco falco-vtf5q -- falco --version
Falco version: 0.30.0
Driver version: 3aa7a83bf7b9e6229a3824e3fd1f4452d1e95cb4
```

```yaml
customRules:
  my_rules: |-
    - required_engine_version: 11
```      

**Rule Priorities**

EMERGENCY, ALERT, CRITICAL, ERROR, WARNING, NOTICE, INFORMATIONAL, DEBUG.

**Rule Tags**

```yaml
- rule: File Open by Privileged Container
  desc: ...
  output: ...
  priority: WARNING
  tags: [container, cis]
```

**Escaping Special Characters**

```yaml
condition: evt.type=open and proc.name="(systemd)" or proc.name=systemd
```

[Supported Syscall Events](https://falco.org/docs/rules/supported-events/)

## Disable default rules

### Disable rules using arguments

- `-D <substring>`: Disable any rules with names having the specified substring. 
- `-T <tag>`: Disable any rules that contains the specified tag.
- `-t <tag>`: Only run those rules that contain the specified tag. 

Eg: disables the "Write below root" rule:

```shell
falco -D Write below root
```

Helm charts,

```shell
--set "extraArgs={-DWrite below root}"
```

### Disable rules using Macros

```yaml
- macro: consider_packet_socket_communication
  condition: (always_true)
```

or user `never_true`.

### Disable rules using Custom Rule Definition

```yaml
- rule: User mgmt binaries
  append: true
  enabled: false
```

then

```yaml
- rule: User mgmt binaries
  append: true
  condition: and (never_true)
```

**Run falco and enable only the rules that contains the tags my_rules, file, or filesystem:**

```shell
helm upgrade falco --set "extraArgs={-tmy_rules,-tfile,-tfilesystem}" falcosecurity/falco -n falco -f my_rules.yaml
```

## Rule Conditions

```yaml
evt.type = execve and evt.dir = < and (proc.name = cat or proc.name = grep)
```

## Falco Rule Exceptions

```yaml
- rule: <the_name_of_the_rule>
  desc: (...)
  condition: (...)
  output: (...)
  tags: (...)
  exceptions:
   - name: <name_of_the_exception>
     fields: [proc.name, fd.name]
     comps: [=, in]
     values:
       - [my_bin_A, [my_file_A]]
       - [custom_bin_B, [allowed_file_for_B, other_B_file]]
```

- `name`: an identifier of the exception.
- `fields`: fields to consider.
- `comps`: operators to match between the fields and values.
- `values`: values to consider.

Sample:

```yaml
customRules:
  custom_rules_from_default: |-
    (...)
    - rule: Run shell untrusted
    (...)
      exceptions:
      - name: app1_shell_runner
        fields: [proc.pname, proc.cmdline]
        comps: [=, =]
        values:
        - [apache2, bash -c ls /root > /tmp/pmt]
    (...)
```

## Exception Syntax shortcuts

Check [doc](https://falco.org/docs/rules/exceptions/#exception-syntax-shortcuts)

## Faclo Alerts

Alerts can be sent to multiple supported channels:

- Standard Output (e.g. stdout, docker logs, kubectl logs)
- syslog
- Files
- input to a program (e.g. mail)

**File output**

This is the default file_output configuration:

```yaml
file_output:
  enabled: true
  keep_alive: true
  filename: /root/falco_alerts_file.txt
```

**Program output**

Send email:

```yaml
program_output:
  enabled: true
  keep_alive: true
  program: mail -s "Falco Notification" someone@example.com
```

```yaml
json_output: true

# (...)

http_output:
  enabled: true
  url: http://some.url/some/path/
```

## Falco exporter

Docker compose

```yaml
version: '3.3'

services:
    falco:
      image: 'falcosecurity/falco:latest'
      privileged: true
      restart: unless-stopped
      container_name: falco
      volumes:
        - '/var/run/docker.sock:/host/var/run/docker.sock'
        - '/var/run/falco:/var/run/falco'
        - '/dev:/host/dev'
        - '/proc:/host/proc:ro'
        - '/boot:/host/boot:ro'
        - '/lib/modules:/host/lib/modules:ro'
        - '/usr:/host/usr:ro'
        - '/etc:/host/etc:ro'
        - '/root/falco.yaml:/etc/falco/falco.yaml:ro'

    falcoexporter:
      image: falcosecurity/falco-exporter
      container_name: falcoexporter
      restart: unless-stopped
      volumes:
        - '/var/run/falco:/var/run/falco'
      depends_on:
        - falco
      ports:
        - 9376:9376
      command:
        - '--client-socket=/var/run/falco/falco.sock'

    prometheus:
      image: prom/prometheus
      container_name: prometheus
      restart: unless-stopped
      volumes:
       - '/root/prometheus.yaml:/etc/prometheus/prometheus.yaml'
      ports:
        - 9090:9090
      depends_on:
        - falcoexporter
      command:
        - '--config.file=/etc/prometheus/prometheus.yaml'
```

## Falco Response Engine



## References

- [Falco 101](https://learn.sysdig.com/falco-101/) - Free course from Sysdig