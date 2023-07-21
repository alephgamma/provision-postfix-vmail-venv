# provision-postfix-vmail-venv

This is an ansible environment to provision an email server with virtual mailboxes.

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
```
  - name: install and configure postfix with virtual mailboxes
  hosts: mail
  become: true
  vars:
    version: postfix
    domain: "{{ lookup('env','DOMAIN') }}"
    # vpath: "/var/mail/vhosts"
    vpath: /var/spool/vmail
    owner:
      username: centos
      uid: 1010
    vusers:
      - "info"
      - "sales"
    vdirs:
      [ "cur", "new", "tmp" ]
```
## The templates

## Use-Cases

### Local submission: Receipt verification from local user to local user </u>
---

### Local submission: Transmission (sent) verification from local user to local user
---

### Network submission: Receipt verification at the target-node
---

### Network submission: Transmission (sent) verification from the target-node
---
