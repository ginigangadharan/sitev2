---
layout: post
title: SSL Certificate Management - Quick Reference
author: gini
categories: [ security]
image: "assets/images/2020/jon-moore-bBavss4ZQcA-unsplash-locks.jpg"
tags: [cloud, automation, containers, kubernetes]
permalink: ssl
featured: false
hidden: false
showindex: true
titleshort: ssl
---

- [Create SSL Certificate](#create-ssl-certificate)
  - [Create a Root CA](#create-a-root-ca)
  - [Create Server Key, CSR and Certificate](#create-server-key-csr-and-certificate)
- [How to verify SSL Certificates](#how-to-verify-ssl-certificates)
  - [Verify Certificate and Key](#verify-certificate-and-key)

## Create SSL Certificate
### Create a Root CA

```bash
## create CA key
## remove the -des3 option for non-password protected key 
openssl genrsa -des3 -out myserver-CA.key  4096

## self-sign CA Certificate
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out myserver-CA.pem
```

### Create Server Key, CSR and Certificate

```bash
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

## How to verify SSL Certificates

### Verify Certificate and Key

You should get the same `md5` output for all commands.

```shell
# certificate
$ openssl x509 –noout –modulus –in <file>.crt | openssl md5

# key
$ openssl rsa –noout –modulus –in <file>.key | openssl md5

# csr
$ openssl req -noout -modulus -in <file>.csr | openssl md5
```

Check the Key only

```shell
$ openssl rsa -check -noout -in myserver.key
RSA Key is ok
```