---
layout: post
title: SSL Certificate Management - Quick Reference
author: gini
categories: [ security]
#image: "assets/images/how-to-create-scheduled-snapshots-in-google-cloud-platform.PNG"
tags: [cloud, automation, containers, kubernetes]
permalink: ssl
featured: false
hidden: false
showindex: true
titleshort: ssl
---

```bash
- create CA and lines below (find CA step from net)
## create CA and sign it.
## emove the -des3 option for non-password protected key 
openssl genrsa -des3 -out myserver-CA.key  4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out myserver-CA.pem

## Create a new SSL Key for server/app
openssl genrsa -out myserver.key 2048

## Generate Certificate Signing Request
$ openssl req -new \
  -subj "/C=US/ST=North Carolina/L=Raleigh/O=Red Hat/CN=todo-https.apps.ocp4.example.com" \
  -key myserver.key \
  -out myserver.csr

## Verify CSR content
$ openssl req -in myserver.csr -noout -text

## Generate Certificate using CSR and CA
## openssl x509 -req -in <CSR FILE> \
##   -CA <CA FILE> -CAkey myserver-CA.key -CAcreateserial \
##   -passin file:passphrase.txt \
##   -out <EXPORT CRT> -days 3650 -sha256 -extfile myserver.ext
$ openssl x509 -req \
  -passin file:passphrase.txt \
  -CA myserver-CA.pem -CAkey myserver-CA.key -CAcreateserial \
  -in myserver.csr \
  -out myserver.crt \
  -days 1825 -sha256 -extfile myserver.ext

## verify certificte content
$ openssl x509 -in myserver.crt -text -noout
```