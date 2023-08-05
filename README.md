# provision-postfix-vmail-venv

This is an ansible environment to provision an email server with virtual mailboxes. Additional improvements add opendkim, spf and opendmarc features.

## Pre-requisites

* ansible control-node
  - mkvirtualenv/postactivate contains environment variables:
    - DOMAIN
    - IP
* target-node
  - Local user(s)
    - centos
* DNS domain
  - Domain record: `{{ your-domain.com }}`
  - Mail server record: `mail.{{ your-domain.com }}`
  - MX record:  `10 mail.{{ your-domain.com }}`
* PTR record
* HTTPS Certificate is ready
* External email user

## The Big Picture
For those of us that learn better by seeing, the image below is for visualization. 
![alt text](https://github.com/alephgamma/provision-postfix-vmail-venv/blob/master/postfix-vmail.png?raw=true)

## The playbooks

### provision_postfix_vmail.yml
```
- name: install and configure postfix with virtual mailboxes
  hosts: mail
  become: true
  vars:
    version: postfix
    domain: "{{ lookup('env','DOMAIN') }}"
    # vpath: "/var/mail/vhosts"
    vpath: /var/spool/vmail
    user:
      name: centos
      uid: 1010
    vusers:
      - "info"
      - "sales"
    vdirs:
      [ "cur", "new", "tmp" ]
  ...
```

## The templates

### postfix/main.cf.j2
### postfix/virtual.j2
### postfix/vmailbox.j2

### opendkim/opendkim.conf.j2
### opendkim/KeyTable.j2
### opendkim/SigningTable.j2
### opendkim/TrustedHosts.j2

### opendmarc/opendmarc.conf.j2

### muttrc.j2

### make_env.py.j2
```
#!/usr/bin/python3

import re,os

d  = '/etc/opendkim/keys/{{ domain }}/{{ date }}.txt'
ef = '/home/{{ mail_user }}/.env'

f=[line.replace('\n',' ') for line in open(d)]

t = re.findall('"([^"]*)"',' '.join(f))

with open(ef,'w') as file:
    file.write('D1='+t[0]+'\n')
    file.write('D2='+t[1]+'\n')
    file.write('D3='+t[2]+'\n')
```

# Use-Cases

## Local submission: Receipt verification from local user to local user 

## Local submission: Transmission (sent) verification from local user to local user

## Local submission to network user

## Network submission: Receipt verification at the target-node

## Network submission: Transmission (sent) verification from the target-node
