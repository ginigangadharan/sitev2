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

- [Create a Root CA](#create-a-root-ca)
- [Trusting new RootCA in system](#trusting-new-rootca-in-system)
- [Create Server Key, CSR and Certificate](#create-server-key-csr-and-certificate)
- [How to verify SSL Certificates](#how-to-verify-ssl-certificates)
  - [Verify Certificate and Key](#verify-certificate-and-key)
  - [Change or remove passhphrase](#change-or-remove-passhphrase)
  - [Extract from PFX file](#extract-from-pfx-file)
  - [Extract Certificate from P7B file](#extract-certificate-from-p7b-file)
- [References](#references)

## Create a Root CA

```shell
$ mkdir RootCA && cd RootCA

# Create RootCA Key
$ openssl genrsa -des3 -out rootCA.key 4096
```

Remove the `-des3` option for non-password protected key.

The -des3 option specifies how the private key is encrypted with a password. Without a cipher option, the private key is not encrypted, and no password is required.

Note: Optionally you can create the RootCA.csr and sign it.

```shell
$ openssl req -new -key ca.key -subj "/CN=MYROOT-CA" -out rootCA.csr
$ openssl x509 -req -in rootCA.csr -signkey rootCA.key -out rootCA.crt
```

```shell
# Create self-sign CA Certificate with 10 years validity
$ openssl req -x509 -new -nodes -key rootCA.key -sha512 -days 3650 -out rootCA.pem
```

## Trusting new RootCA in system

Eg: Fedora workstation

```shell
$ sudo cp ../RootCA/rootCA.pem /etc/pki/ca-trust/source/anchor
s/iamgini-rootCA.pem
$ sudo update-ca-trust
```


## Create Server Key, CSR and Certificate

```shell
$ cd .. && mkdir SSL_CERTS  && cd SSL_CERTS
```

Create a new SSL Key for server/application

```shell
$ openssl genrsa -out myserver.key 4096
```

Note: Optionally generate Certificate Signing Request and Key only

```shell
$ openssl req -newkey rsa:4096 \
  -keyout server.key \
  -out server.csr
```

Generate Certificate Signing Request with details as arguments

```shell
$ openssl req -new \
  -subj "/C=SG/ST=Singapore/L=CBD/O=iamgini/CN=aap.lab.iamgini.com" \
  -key myserver.key \
  -out myserver.csr
```

Generate `myserver.crt` Certificate using CSR and CA

```shell
$ openssl x509 -req \
  -CA ../RootCA/rootCA.pem \
  -CAkey ../RootCA/rootCA.key \
  -CAcreateserial \
  -in myserver.csr \
  -out myserver.crt \
  -days 1825 -sha512
```

Verify certificte content

```shell
$ openssl x509 -in myserver.crt -text -noout
```


Check a PKCS#12 file (.pfx or .p12)

```shell
$ openssl pkcs12 -info -in keyStore.p12
```

Check and verify Key file

```shell
$ openssl rsa -in server.key -check
```

Verify CSR content

```shell
$ openssl req -in server.csr -noout -text
$ openssl req -in server.csr -noout -text -verify
```

```shell
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

### Change or remove passhphrase

Remove Passphrase from SSL key

```shell
$ openssl rsa -in original.key -out new.key
```

Change the passphrase of the SSL Key

```shell
$ openssl rsa -aes256 -in original.key -out new.key
```


### Extract from PFX file

```shell
$ openssl pkcs12 -in [yourfile.pfx] -nocerts -out [drlive.key]
```

### Extract Certificate from P7B file

```shell
$ openssl pkcs7 -inform PEM -outform PEM -in certnew.p7b -print_certs > certificate.cer
```


## References

- [OpenSSL Commands](https://pleasantpasswords.com/info/pleasant-password-server/b-server-configuration/3-installing-a-3rd-party-certificate/openssl-commands)


```shell
 # openssl genrsa -out pgsql.key 4096

 # vi pgsql_csr.conf

   [req]
   default_md = sha512
   prompt = no
   req_extensions = req_ext
   distinguished_name = req_distinguished_name
   [req_distinguished_name]
   commonName = database.ansible.com
   countryName = US
   [req_ext]
   subjectAltName = @alt_names
   [alt_names]
   DNS.0 = database.ansible.com
   IP.0 = 192.168.7.19

 # openssl req -new -nodes -key pgsql.key -config pgsql_csr.conf -out pgsql.csr

 # openssl x509 -req -in pgsql.csr -days 3650 -CA ~/RootCA/rootCA.pem -CAkey ~/RootCA/rootCA.key -CAcreateserial -extensions req_ext -out pgsql.crt -extfile pgsql_csr.conf

  please transfer the rootCA.pem to the Controller node

 # cp rootCA.pem /etc/pki/ca-trust/source/anchors/
 # restorecon -Rv /etc/pki/ca-trust/source/anchors/
 # update-ca-trust

```