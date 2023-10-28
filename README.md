# provision-postfix-vmail-venv

This is an ansible environment to provision an email server with virtual mailboxes. Additional improvements add opendkim, spf and opendmarc features with the respective DNS records.

## 0. Pre-requisites

* control-node (osm-c9)
  - Local user
    - `ec2-user` with key-pair
* target-node (email server)
  - Local user(s)
    - `centos`
* DNS domain
  - Domain record: `{{ your-domain.com }}` - Probably already present
  - Mail server record: `mail.{{ your-domain.com }}` - Probably already present
  - MX record:  `10 mail.{{ your-domain.com }}` - Probably already present
* PTR record - Provisioned by the VPS
* HTTPS Certificate is ready
* External email user

## 1. The Big Picture
For those of us that learn better by seeing, the image below is for visualization. 
![alt text](https://github.com/alephgamma/provision-postfix-vmail-venv/blob/master/postfix-vmail.png?raw=true)

## 2. Create, configure and enable the virtual environment

* Make the virtualenv on the control-node
  - mkvirtualenv **YOUR-PROJECT**
* Configure the environment
  - Edit: ```$HOME/.virtualenvs/provision-postfix-vmail-venv/bin/postactivate```
  - postactivate exports the environment variables:
    - DOMAIN
    - IP
    - VENV: The name of **YOUR-PROJECT**
    - ANSIBLE_CONFIG
    - AWS_ACCESS_KEY_ID
    - AWS_SECRET_ACCESS_KEY
    - MAIL_USER: The user on the target-node
  - postactivate finally runs:
    - `cd /home/ec2-user/provision-postfix-vmail-venv`
* Enable the virtual environment
  - workon **YOUR-PROJECT**

## 3. The playbooks

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
### make_env.yml
### provision_mutt_for_user.yml
### provision_route53.yml

## 4. The templates

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

# 5. Use-Cases

## Local submission: Receipt verification from local user to local user 

## Local submission: Transmission (sent) verification from local user to local user

## Local submission to network user

## Network submission: Receipt verification at the target-node

## Network submission: Transmission (sent) verification from the target-node
