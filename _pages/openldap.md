---
layout: post
title: Installing OpenLDAP
author: gini
categories: [ GNS3 ]
image: "assets/images/2020/photo-1456428746267-a1756408f782-@clintadair.jpg"
tags: []
show-avatar: false
permalink: openldap
featured: false
hidden: false
titleshort: openldap
---

# Install LDAP Server

## Install Packages

```
yum install *openldap* -y
```

## Start Service

```
# systemctl start slapd
# systemctl enable slapd
```

## enable firewall port
```
# firewall-cmd --add-service=ldap 
```

# Configure LDAP Server

## Update admin password

```
# slappasswd
New password: 
Re-enter new password:
{SSHA}8hG0fZ8xxDE8tcp2g0YAhurCy3qIZI+I
```

## create an LDIF file (ldaprootpasswd.ldif) which is used to add an entry to the LDAP directory.

```
# vim ldaprootpasswd.ldif

# cat ldaprootpasswd.ldif 
dn: olcDatabase={0}config,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}8hG0fZ8xxDE8tcp2
```

- `olcDatabase`: indicates a specific database instance name and can be typically found inside /etc/openldap/slapd.d/cn=config.
- `cn=config`: indicates global config options.
- `PASSWORD`: is the hashed string obtained while creating the administrative user.

## Add entry from file

```
# ldapadd -Y EXTERNAL -H ldapi:/// -f ldaprootpasswd.ldif 
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "olcDatabase={0}config,cn=config"
```

# Configure LDAP DB

## Copy Sample DB
```
# cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
# chown -R ldap:ldap /var/lib/ldap/DB_CONFIG 
# systemctl restart slapd
```

## import some basic LDAP schemas from the /etc/openldap/schema directory as follows.

```
$ sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif 
$ sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
$ sudo ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```

## Add domain info

```
# cat ldapdomain.ldif 

dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth"
  read by dn.base="cn=Manager,dc=lab,dc=com" read by * none

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=lab,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=lab,dc=com

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcRootPW
olcRootPW: {SSHA}8hG0fZ8xxDE8tcp2

dn: olcDatabase={2}hdb,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword,shadowLastChange by
  dn="cn=Manager,dc=lab,dc=com" write by anonymous auth by self write by * none
olcAccess: {1}to dn.base="" by * read
olcAccess: {2}to * by dn="cn=Manager,dc=lab,dc=com" write by * read
```

Update DB
```
# ldapmodify -Y EXTERNAL -H ldapi:/// -f ldapdomain.ldif 
```

## Create `baseldapdomain.ldif` with more details

```
dn: dc=example,dc=com
objectClass: top
objectClass: dcObject
objectclass: organization
o: example com
dc: example

dn: cn=Manager,dc=example,dc=com
objectClass: organizationalRole
cn: Manager
description: Directory Manager

dn: ou=People,dc=example,dc=com
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=example,dc=com
objectClass: organizationalUnit
ou: Group 
```

and 
```
# ldapadd -Y EXTERNAL -x -D cn=Manager,dc=example,dc=com -W -f baseldapdomain.ldif
```

# Configure Client using SSSD

```
## Install packages
yum install sssd sssd-client

## Configure System
authconfig --enablesssd --enablesssdauth --ldapserver="<ldapserver>" --ldapbasedn="<ldap-base-dn>" --enableldaptls --update

## eg:
authconfig --enablesssd --enablesssdauth --ldapserver="ldap.jumpcloud.com" --ldapbasedn="ou=Users,o=5ffd6b642482a16659721d5c,dc=jumpcloud,dc=com" --enableldaptls --update
```


# Appendix

**[Reference](https://www.howtoforge.com/linux_openldap_setup_server_client)**
https://www.tecmint.com/install-openldap-server-for-centralized-authentication/